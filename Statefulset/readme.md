In the world of Kubernetes, most applications (like web servers or APIs) are stateless—they don't care which pod they are running on, and if one dies, a new one takes its place with no memory of the past.

However, databases (PostgreSQL, MongoDB) and distributed systems (Kafka, ZooKeeper) are stateful. They require a persistent identity.

# What is a StatefulSet?
A StatefulSet is the Kubernetes workload object used to manage stateful applications. Unlike a Deployment, a StatefulSet maintains a sticky identity for each of its Pods. These pods are created from the same spec but are not interchangeable; each has a persistent identifier that it maintains across any rescheduling.

# Why do we need it?
Stable, Unique Network Identifiers: Pods are named hostname-0, hostname-1, etc. If db-0 crashes, it restarts as db-0.

Stable, Persistent Storage: Each Pod gets its own Dedicated Persistent Volume (PV). If the Pod moves to a different node, the storage follows it.

Ordered Deployment/Scaling: Pods are created and terminated in a strict order (e.g., 0, then 1, then 2). This is critical for database clusters where a "Primary" must exist before "Secondaries."

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