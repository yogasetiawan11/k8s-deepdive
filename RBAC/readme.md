# The difference between RoleBinding and ClusterRoleBinding.
RoleBinding is namespace, meaning only where role binding is defined that permission are valid whereas ClusterRoleBinding is at the cluster level they can access the resources in any namespace

# Groups 
Group let us to create and handle hundred of users at the cluster, using group instead of defining user separately we assign a user to a group instead of giving them to users 

# ServiceAccount
Service account are special types of users you can call them as application users. When a namespace gets created default service account is created in every namespace, when our application runs use this default service accounts to authenticate themselve with API server. If we dont't mention specifically mention what service account we want to use, All parts will use default service account. 

List all service account in the cluster
```sh
k get sa
```

Create a new SA
```sh
k create sa name-sa
```

[Attach a ServiceAccount with a Role](RBAC/ServiceAccount.yaml)

this Command bellow to verify the rule of your SA:

```sh
kubectl auth can-i create pods --as="system:serviceaccount:default:SA-name"
```
