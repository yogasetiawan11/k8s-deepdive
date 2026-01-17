# 3 type of Autoscallers HPA, VPA, CA
# Horizontal Pod Autoscaller
HPA Increases number of Replicas whenever there's a spike in CPU, Memory or some other metrics, that means the Load are distributed along the pods
# Vertical Pod Autoscaller
Increasing number a pods isn't always solution, for instance we can make our database handle more connections by increasing memory and CPU. in such cases we need to increas the load of resources of existing pods instead of creating new pods. with VPA wwe can analyze a resources of deployment and adjust them accordingly to handle the load.

In VPA there are 3 type of Update policy e.g Off, Initial, Auto: 

- **Auto**: This mode means the VPA will directly apply the suggested recommendations by updating the pods.

- **Off**: It just gives recommendations but does not automatically update the replicas. This mode is generally preferred in production environments because automatic updates can cause workload disruptions due to pod restarts

- **Initial**: This mode applies the recommended resource values only when new pods are created


By default VPA not installed, you can install it by follow this command: 
```sh
git clone https://github.com/kubernetes/autoscaler.git
```

```sh
cd autoscaler
```

```sh
./vertical-pod-autoscaler/hack/vpa-up.sh
```

then apply this [VPA file](Auto-Scalling/VPA.yaml)


# Cluster Autoscaller
CA add the node to the cluster If there are any pods that stuck in pending state because lack of resources in the cluster. Not only scalling up these 3 Auto Scaller can also scalling down also.