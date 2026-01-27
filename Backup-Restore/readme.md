# prerequisite
- Kubernetes Cluster
- AWS
- S3 Bucket
- (AWS-CLI)[https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html] 
- (Kubectl)[https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/]

# Install Velero
There are 2 component ``Client component`` and ``Server component`` Install CLient component on ``Local machine``, Install Server component on Kubernetes ``Manager``. 

to Download these components refers to this [documentation](https://velero.io/docs/v1.0.0/aws-config/).


## 1. Install Velero Client
[Install velero latest version](https://velero.io/docs/v1.0.0/aws-config/)

- Download Velero 
```sh
wget https://github.com/vmware-tanzu/velero/releases/download/v1.17.1/velero-v1.17.1-linux-amd64.tar.gz
```

Extract the Velero File.gz
```sh
tar -xvf velero-v1.17.1-linux-amd64.tar.gz
```

Change Directory to Velero File
```sh
cd velero-v1.17.1-linux-amd64
```

Copy Velero File on to /bin path
```sh
sudo cp velero /usr/local/bin/
```

Verify If Velero command has configured
```sh
velero --help
```

# Create S3 Bucket
```sh
BUCKET=<YOUR_BUCKET>
REGION=<YOUR_REGION>
aws s3api create-bucket \
    --bucket $BUCKET \
    --region $REGION \
    --create-bucket-configuration LocationConstraint=$REGION
```

## 2. Create & Configure Velero user
```sh
aws iam create-user --user-name velero
```

## Attach policies to give velero the necessary permissions:
```sh
cat > velero-policy.json <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeVolumes",
                "ec2:DescribeSnapshots",
                "ec2:CreateTags",
                "ec2:CreateVolume",
                "ec2:CreateSnapshot",
                "ec2:DeleteSnapshot"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:PutObject",
                "s3:AbortMultipartUpload",
                "s3:ListMultipartUploadParts"
            ],
            "Resource": [
                "arn:aws:s3:::${BUCKET}/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::${BUCKET}"
            ]
        }
    ]
}
EOF
```

Attach a policy to the Velero user
```sh
aws iam put-user-policy \
  --user-name velero \
  --policy-name velero \
  --policy-document file://velero-policy.json
```

## 3. Create an Access key for the velero user
```sh
aws iam create-access-key --user-name velero
```
The result should like this
```sh
{
    "AccessKey": {
        "UserName": "velero",
        "Status": "Active",
        "CreateDate": "2017-07-31T22:24:41.576Z",
        "SecretAccessKey": "<AWS_SECRET_ACCESS_KEY>",
        "AccessKeyId": "<AWS_ACCESS_KEY_ID>"
    }
}
```
then store your access key id to velero
```sh
echo <AccessKeyID> > velero-creds
```

## 4. Create a Velero-specific credentials file ``(credentials-velero)`` in your local directory:
vim credentials-velero then paste this script below 
```sh
[default]
aws_access_key_id=<AWS_ACCESS_KEY_ID>
aws_secret_access_key=<AWS_SECRET_ACCESS_KEY>
```

Note: Replace the placeholders with the access key ID and secret values returned from the create-access-key request.

---

# Install Velero Manager
Run the following command to install Velero with the AWS plugin:
```sh
velero install \
  --provider aws \
  --plugins velero/velero-plugin-for-aws:v1.9.0 \
  --bucket $BUCKET \
  --secret-file ./credentials-velero \
  --backup-location-config region=$REGION \
  --snapshot-location-config region=$REGION
```

verify If the velero has existed 

```sh
kubectl -n velero get all
```

## Test disaster recovery
deployment.yaml
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
```sh
kubectl apply -f deployment.yaml -n dev
```

## Take a backup of the deployment
```sh
velero backup create dev-backup --include-namespaces dev
```
It will automatically store the backup to S3 backup
## Delete and Restore 
```sh
kubectl delete deploy -n dev
```

### Get list all backup
```sh
velero get backups
```

### Restore the deployment
```sh
velero restore create --from-backup dev-backup
```
``dev-backup`` is a backup name in S3 bucket
