# The difference between statefulSet and Stateless
## Stateful Applications

Stateful applications store the state of the current request, and subsequent requests depend on the state of previous ones.

An example is a simple Spring Boot application where an authentication flag is stored in memory. If multiple instances are running and a user is authenticated on one instance, another instance will not have that authentication flag set in memory. This can lead to inconsistent or incorrect results.

Databases are considered stateful applications because they store and maintain data over time.

## Stateless Applications

Stateless applications do not store any state internally.

Instead, the state is typically moved to an external system such as a database.

An example is a Spring Boot application that generates and validates a token stored in a database for subsequent requests. Because the authentication state is stored externally, any instance of the application can validate the request, ensuring consistent results regardless of how many instances are running.

# Example
Kubernetes StatefulSet Demo (PostgreSQL)
This guide demonstrates how to deploy a stateful PostgreSQL instance to verify that data persists even if the Pod is deleted.

1. The Headless Service
StatefulSets require a "Headless Service" to control the network domain of the Pods.

YAML

# postgres-service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres-service
  labels:
    app: postgres
spec:
  ports:
  - port: 5432
    name: postgres
  clusterIP: None # This makes it "Headless"
  selector:
    app: postgres
```
2. The StatefulSet Manifest
This includes a volumeClaimTemplates section, which instructs Kubernetes to provision a unique disk for every Pod created.

# postgres-statefulset.yaml

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: pg-db
spec:
  selector:
    matchLabels:
      app: postgres
  serviceName: "postgres-service"
  replicas: 1
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres:15
        env:
        - name: POSTGRES_PASSWORD
          value: "mypassword"
        ports:
        - containerPort: 5432
          name: postgres
        volumeMounts:
        - name: pg-data
          mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:
  - metadata:
      name: pg-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```
3. Execution & Verification
- Step A: Deploy the resources
```Bash

kubectl apply -f postgres-service.yaml
kubectl apply -f postgres-statefulset.yaml
```
- Step B: Create data in the Pod
Exec into the pod and create a dummy table.

```Bash

kubectl exec -it pg-db-0 -- psql -U postgres -c "CREATE TABLE devops_test (id SERIAL PRIMARY KEY, task TEXT);"
kubectl exec -it pg-db-0 -- psql -U postgres -c "INSERT INTO 
devops_test (task) VALUES ('Check Persistence');"
```
- Step C: Simulate a Failure (Delete the Pod)
```Bash

kubectl delete pod pg-db-0
```
Wait for Kubernetes to restart the pod. Because it is a StatefulSet, the new pod will be named pg-db-0 and will re-attach to the exact same disk.

Step D: Verify Data Survival
```Bash

kubectl exec -it pg-db-0 -- psql -U postgres -c "SELECT * FROM devops_test;"
```

You will see the "Check Persistence" row, proving that the state survived the pod destruction.