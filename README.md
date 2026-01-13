# Kubernetes Advanced Operations & Resource Management
Welcome to the second part of my Kubernetes journey! While my first repo covers the fundamentals, this repository is dedicated to Day 2 Operations, Scaling Strategies, Stateful Workloads, and Advanced Scheduling.

This repo serves as a practical lab for managing production-grade Kubernetes clusters with a focus on efficiency, reliability, and automation.

# 🚀 Topics Covered
1. Scalability (Auto-scaling)
Learn how to make your cluster elastic based on demand.

- HPA (Horizontal Pod Autoscaler): Scaling pods based on CPU/Memory metrics.

- VPA (Vertical Pod Autoscaler): Automatically adjusting pod resource requests/limits.

- Cluster Autoscaler: Automatically adding/removing nodes from the cloud provider.

2. Resource & Scheduling Deep Dive
Understanding how Kubernetes decides where a pod should live.

- Resource Management: Fine-tuning requests and limits.

- Advanced Scheduling: Node Affinity/Anti-affinity, Taints and Tolerations.

- Priority & Preemption: Handling high-priority workloads.

3. Stateful Workloads & Storage
Managing applications that require data persistence.

- StatefulSets: Managing stateful apps like databases (MySQL, MongoDB).

- PV, PVC, & StorageClasses: Decoupling storage logic from the pod lifecycle.

4. Workload Controllers (Advanced)
Beyond simple Deployments.

- DaemonSet: Ensuring a pod runs on every node (logging, monitoring agents).

- Jobs & CronJobs: Running batch processes and scheduled tasks.

5. Health & Reliability (Probes)
Keeping the application healthy.

- Liveness Probes: Restarting unhealthy containers.

- Readiness Probes: Managing traffic flow to pods.

- Startup Probes: Handling slow-starting legacy apps.

# 📂 Repository Structure
```Bash

.
 []
└── 01-probes/
    └── startup probe/
    └── liveness probe/
    └── readiness probe/
└── 02-resource management/
    └── startup probe/
├── 03-storage/
│   ├── storage-classes/
│   ├── persistent-volumes/
│   └── statefulsets/
├── 04-scheduling/
│   ├── affinity-anti-affinity/
│   ├── taints-tolerations/
│   └── priority-classes/
├── 05-autoscaling/
│   ├── hpa/
│   ├── vpa/
│   └── cluster-autoscaler/
├── 05-workloads/
│   ├── daemonsets/
│   ├── jobs/
│   └── cronjobs/
```

# 🛠️ Prerequisites
- Kubernetes Cluster (Minikube / Kind / Managed Service like GKE/EKS).

- kubectl installed and configured.

- Metrics Server installed (Required for HPA/VPA).

# 📖 How to Use This Repo
Clone the repo:

```Bash

git clone https://github.com/yourusername/k8s-advanced-ops.git
Navigate to a topic:
```
```Bash

cd 01-autoscaling/hpa
Apply the manifest:
```
```Bash

kubectl apply -f .
```
Author: Yoga setiawan | Links to my fundamental k8s repo: [here's the repo](https://github.com/yogasetiawan11/Kubernetes)
