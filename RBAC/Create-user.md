1. Create a user in Kubernetes 
- open file ``vim .kube/config`` inside this file there's a Information about the user of the cluster 

```yaml 
apiVersion: v1
# This is information about Clusters that you can access
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJR2R4QnlMc1RXdHd3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TmpBeE1EUXdOelUyTXpsYUZ3MHpOakF4TURJd09EQXhNemxhTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUUREdndieThnSFhkV0ZEbWZ4ZGFqL0ZYaTdqSUUwS2RQb3BDVTFwb2FHb3lUM0NxR0FsMlNocmJwOEgKSUVpTzNQVysvZnA1ZUhnMEEzYWs3cGpENVlwTXZDT3dnVFpIWDR5TjBtS3dGcEwyd3RydG8rakRnRSs4dk0rQwpGS2hKVVdOY1Q1RmZNT1EyVVg4bWtLSkdtVzA4aSt1a0ZnZXB0YWtjWW9pckFZbWJaS3ZNM05VOFNTUUtRK2M0ClFBcjV5Q0lJaHY2NHZCTGpuY1ZhQUxUcldtYjdzcW53ODlLK3k1SlExNEZ1ZEpXM1FjemxVSWF3eWIxTGFybTgKVmZmSmplVm5EZHFkelZHSWVWUHA2dFdvdVlyaWg5R0VNRFEzdEM5NUFkS0RvQ05vRjJaVDNpa0FvTldBaTlRRQpQOUQ4UDdSajJOaHFJb0FaeEI4YlVxQUcrRU81QWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJUamxVRXhKVG84NFNQeWlLU3YwMlhxN29PRmZEQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ1NuRnl0bGNrSAo0emJPTjBrTmMzTmk3Qmt6Qi9IY3o3V05KNEV1NHY5ZXFiTFZPc1Yybk9nYis2cUpOaWplNEVZN2NGWDB5Q1M5Cms1WGRUb0ZvRkFHcUZzMEZtM1ZicjFFcERRT3l1dUxhYXNaWDVQY1cxb2tKSDVZUlQ3K0hZdVowbXhCSlFMdXIKeTV4WGRMYjBnOGlZaHpWcVdkcDZWa2J3Q0FwOFNlM1lkbmZ0WnNUbjRyakFpZmp6SlExUDUwZFd1VEw4RUNYWgp1RXJNMmJqaHlvakhJSHRoaXBLbVJ3SGdHYkpGQlBiSHhLb1JGbGtPQms4blFQSHZHNTlhZnZaaVRsaG9sVGIyCnpOWm5uU1poOVRkeTVYa1h1UjRHa2NMVDFFMnJWQnFJc00veVVkSGFNUXRQMzJBMWhjRXN3NzMyNWREMm1JN0QKQWI5RGN6TXRvTTZzCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://controlplane:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}

# Here is List of users of this Kubernetes cluster
users:
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURLVENDQWhHZ0F3SUJBZ0lJS2NvVUxNOC9zdHN3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TmpBeE1EUXdOelUyTXpsYUZ3MHlOekF4TURRd09EQXhNemxhTUR3eApIekFkQmdOVkJBb1RGbXQxWW1WaFpHMDZZMngxYzNSbGNpMWhaRzFwYm5NeEdUQVhCZ05WQkFNVEVHdDFZbVZ5CmJtVjBaWE10WVdSdGFXNHdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCRHdBd2dnRUtBb0lCQVFEdkQ2czAKNmtsdFZBK3BGU1l0VytsZHpDZU4yTWFRTTYwYVpoemEwRnFqR25XUWFkTENOZ1hvZS9vNDgybHZxRkhaeGpkMQp4d1ovQ1pqck5nQnVPQzM2cHUxUTA5Mmt2WGsvZXVXMU9nc2hqcXhENHh1YnZ4VzZkaWdGRGJ0ekNhRVF1cGI1Ci9XQkEzUk5Yd0pHS3F0K0M1ZHJHTjNndW4xUVBrMlNnbzNHb21LYTJkaGNpWUJ1NzRuRmNDaVR5V3d5YURMR2UKYkV1ZFc0OERvMjhORzZVcWxDRllKWjlBTW45dFpsbHJrT2FmSVFRTHJzL3Z6V1l6RENtbTdEdEEyRGhUeG1aZQp6emxpV2lSVmErMnZvMVRkK0wrVEVtU3d6MExqdHlJOHNGQ1VjTy9xT2owaXdFMlBjdzQzRFFqVU80NXNVMXB4CjV1S0lDK3U3Y0dXNklGN3hBZ01CQUFHalZqQlVNQTRHQTFVZER3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUsKQmdnckJnRUZCUWNEQWpBTUJnTlZIUk1CQWY4RUFqQUFNQjhHQTFVZEl3UVlNQmFBRk9PVlFURWxPanpoSS9LSQpwSy9UWmVydWc0VjhNQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUJBUUJFT2oxZW9VOGtVbzFFRHpjNyt5RVlyYW10CjNqVmVyMFlGM3N5azZEV0FTTWl6TmpLZ1hEa0dsb2lscjE4MXhNc0ZNdTdRamVUd1p4ZFJQWXdMZ1l0TS9mQloKbFBwV2FMSEs1QlptR3BjRG9HM2J6Um9HcEpYbHVHOTFpamgrQU83bGpNMjdlcVVRUGxoV09vTmlIWHY3T0liYwo4U2xXOVVMS2NSS2lhSnliSHU3eWRnUHFSdFVlMlNWajVKdGVmTjVhMklPdffdsL0VjunedsfejUxTVk0R2ZCSk9pemNXbTNJCmU3RnVkZ2ZiQmZFdzltbThYS1ZoakpJay9NQUEwRDA2N1JOQzcvaHo1MVdFVyt2cHo1VlR0Y0MrYi9VZzJOZlMKTUgzMXBlMVRvWC9WaW9hOXFBZml6MzF0NjNIaEVEOEpRdjB6WjFCb3ZqdkUyYW5QSlBpdUt3aUg4T3JVCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    client-key-data: LS0tLS1C223TiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb3dJQkFBS0NBUUVBN3ckskksdfk5PcEpiVlFQcVJVbUxWdnBYY3duamRqR2tET3RHbVljMnRCYW94cDFrR25TCndqWUY2SHY2T1BOcGI2aFIyY1kzZGNjR2Z3bVk2ellBYmpndCtxYnRVTlBkcEwxNVAzcmx0VG9MSVk2c1ErTWIKbTc4VnVuWW9CUTI3Y3dtaEVMcVcrZjFnUU4wVFY4Q1JpcXJmZ3VYYXhqZDRMcDlVRDVOa29LTnhxSmltdG5ZWApJbUFidStKeFhBb2s4bHNNbWd5eG5teExuVnVQQTZOdkRSdWxLcFFoV0NXZlFESi9iV1paYTVEbW55RUVDNjdQCjc4MW1Nd3dwcHV3N1FOZzRVOFptWHM4NVlsb2tWV3Z0cjZOVTNmaS9reEprc005QzQ3Y2lQTEJRbEhEdjZqbzkKSXNCTmozTU9OdzBJMUR1T2JGTmFjZWJpaUF2cnUzQmx1aUJlOFFJREFRQUJBb0lCQUZmZ242UGM5ODdybHJJbAo1eG5IYnBxZlFHOEJIOFRFYWo2SkNOUmF3T2F5QkVObzB5TzNMaWJPNUNWcFBQbmhtdXo5MDBkRk9Sc2IwUTJ0CmxTenZFOS9PMnUvYVllQjhFZ1VHelVmNEpUMVpyL05vbWE1LzZLSFphMDZvUko2K0RHb1UzUWphWGphWnpkRjEKSlVzZkV2aVFQQUtmUTF1SUVDT0ZuL2o0Ky8vb0g0S2M2NHc0NCs1L1dIcXgrZFJVdElNdktkcmQrNXo3Z2dPYwp4MUFvbGRKMTVsdEhVRFN5NzhVa2FIQjVPRTRlOVdoWE1UMDR6andrOFRGaVVsY3JvNHdEVkJDNUVCVUFiRUFBCndEUnk0Umt3aXAyb2dWaTVEQzdnSi91OEhLbFppV3gyYUlWSDFYV1U0d1VwSytwYXJnT0t1REptQ2MydjNKUVMKb05icVFVMENnWUVBODdzOGg2OU5xUU83V0pXSHcySERsaXJqVUpPaWZtekl5dXBFK3hWWWJabTNBM0FEMWEwWApoUE5BbXhGdk1sQ3pUcEZFMVNFTUgzTXdkN09heDdydHhjV2xnait4enVSaUYrdUdXZStENXNqSjBQZkk5dG1GClVKQ1dMQnNnVE5DU2lHWUNPd1NOczkrLy9wc0l2V0tOMlorT091RDhEK0JZYlZQa3VNZk5yRGNDZ1lFQSt4aEEKWkRMYUsxRU5mTXBaNlFtR1ZhY3RQR3hmUERGMjhqb1creFUxbHNSZmExeEsybXJVZFBIOVk1ZnRlbURjelZJUgo1RHVadUZ0SEZPa1JvZGFYYW41VUVSK3FocE92RXhKbU1sTmtqelJWUTNJSXNYR01PV3ArRjY0Q3BkZ2tTK05PCkxORnJGOGo4anFXeUE0MFJKNUt3WjVja1R6V3h2NHQ3V0FFU1NoY0NnWUJ3ZDBpU0Q3bFZNU3lrenJNTDNEUGwKT2pzVU5sdTMzTGkyc1cxTk11ZFFBNnNvZ2VxekVhRVZyeTF6b0pMZjg4OFpoUHp2SDhXNVNXem0vMUIvczJqKwpacHBkeE1obWdJb25JWDRvUjlaa2l1aGRiY2trNXZDV1lYRjZQcllqMithUjNBaFJkV054eWVDTk9ycklzUTVsCmlqT1dSYlRxR29xVFFDLzlkKzBXOXdLQmdIaXc3bnA3Q3V6Wk43OXMwQXk0WEU2ZFhadjJoMHc2aG03bHh4Z0cKMk14UU5ZRTRTbTV6L1F6OUtBdVFBa0RaZ0NoY3MyYmQwd1NQTXpwMDBObldlTTlpUzJ3enFWYW9jL1daMlcrRgpNQWU1WXVaWlVKNWg4c0hDVXp0MGs2YzluaXl2NUdxY2VucGpUQ29Rc09FT0ZGbk9JMmFYZW9kc1NyVEVDWlNDCloxVWZBb0dCQUlQQUc3ZVcyU04zMm1ZaUhPaHhPK3VuVkZydTJYb050dWk5Rzk5TE5PVFExVDNFVFRoWWhCQVcKcjV0aEdaWE5RK1k0N0Y1MXBMdDlXdEpEaWtTeEFkaHR2YWw4aDJPY0NUT2drM05odW5EK1lISlhSSlowamlVNwo5d2NDbWpPNGNXSmdDZGJCdGQ5Nmhwd2dodmF0VEY0V3ZrU0d5dVhIZ2F5QjE2UTdpc0l2Ci0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==
```

2. Create a User in Kubernetes
```bash
openssl genrsa -out yoga.key 2048
```
It will create Private key of new user

3. Create a Certificate signing request
```bash
openssl req -new -key yoga.key -out yoga.csr -subj "/CN=yoga/O=dev/O=example.org"
```

- -key yoga.key   -> we provide private key that we generated
- CN  -> common name which acts as a username 
- O  -> Group name which can select multiple group to the current user
- -out yoga.csr  -> I sign this certificate to this "-out yoga.csr" file

4. Manually create a new user identity for a Kubernetes cluster.

```bash
openssl x509 -req -CA /etc/kubernetes/pki/ca.crt -CAKey /etc/kubernetes/pki/ca.key -CAcreateserials -days 730 -in yoga.csr -out yoga.crt 
```

If fails run this

```bash
openssl x509 -req \
  -in yoga.csr \
  -CA /etc/kubernetes/pki/ca.crt \
  -CAkey /etc/kubernetes/pki/ca.key \
  -CAcreateserial \
  -out yoga.crt \
  -days 730
```

5. Add the user to the cluster

```bash

kubectl config set-credentials yoga --client-certificate=yoga.crt --client-key=yoga.key
```

6. Ensure If the user has added
```sh
cat .kube/config
```

```sh
output:

- name: yoga
  user:
    client-certificate: /root/yoga.crt
    client-key: /root/yoga.key

```

7. Create a Context for the user
```bash
kubectl config set-context yoga-cluster --cluster=minikube --namespace=default
```
- yoga-cluster -> name the context 
- --cluster=minikube (in this case we use this cluster "kubernetes-admin") -> deploy on to minikube cluster --user=yoga

8. Verify the cluster
```bash
kubectl config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   
          yoga-cluster                  minikube                        default
```

# Role and RoleBinding
## Role
In Kubernetes, a Role defines what actions (verbs) can be performed on specific resources within a single namespace

exam of the role:
```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]  # Define a rule for this Resources
  verbs: ["get", "watch", "list"]   # Define an action
```
To check the verbs of the pods run this command:
```sh
kubectl api-resources -o wide | grep pod
```
and to check all the roles that exist on the cluster run this command: 
```sh
k get roles
```
## RoleBinding
In Kubernetes, a RoleBinding is a configuration file used to grant specific permissions to users, groups, or service accounts within a single namespace. It acts as the "bridge" that connects a Role (which defines what actions can be taken) to a Subject (the identity performing those actions).

# ClusterRole and ClusterRoleBinding
When we have multiple ``ns`` defining a RoleBinding for each ns is a tedious process. for few resources like persistentVolume are not even Namespace which can not add a RoleBinding, so there must be a way to define a Roles and RoleBinding at the Cluster level instead of namespace level, that where ClusterRole and ClusterRoleBinding comes into the picture. 

The difference between ``ClusterRole and ClusterRoleBinding`` and ``Role and RoleBinding`` RoleBinding is a namespace, ClusterRoleBinding at the cluster level meaning they can access the Resoureces in any namespace 