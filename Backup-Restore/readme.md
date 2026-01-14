# prerequisite
- Kubernetes Cluster
- AWS
- S3 Bucket
- (AWS-CLI)[https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html] 
- (Kubectl)[https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/]

# Install Velero
There are 2 component ``Client component`` and ``Server component`` Install CLient component on ``Local machine``, Install Server component on Kubernetes ``Manager``. 

to Download these components refers to this (documentation)[https://velero.io/docs/v1.0.0/aws-config/].


## Install Velero Client
(Install velero latest version)[https://velero.io/docs/v1.0.0/aws-config/]

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

# Create & Configure Velero user
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

```sh
aws iam put-user-policy \
  --user-name velero \
  --policy-name velero \
  --policy-document file://velero-policy.json
```