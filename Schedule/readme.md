In this section we will learn how Kubernetes scheduler works, and also we will learn how we can customize this behaviour using **nodeName**, **nodeSelector**, **Affinity**, **Taint and Tolerations**


# What is scheduler
The Kubernetes scheduler, also known as kube-scheduler, is responsible for assigning pods to suitable nodes within a cluster

# How Kubernetes works? 
Kubernetes Scheduling Process
- Filtering Nodes:
When a pod is created, the scheduler first filters out nodes that cannot run the pod based on its resource requirements and any other defined customizations.

- Scoring Feasible Nodes: 
It then runs a set of functions to score the remaining feasible nodes from 1 to 10.

- Picking the Best Node: 
Finally, it selects the node with the highest score and updates the pod's ``nodeName`` to that chosen node.


# nodeName
example of deploy a pod on to [specific node](todo-app-exam.yaml)

Direct Node Assignment with nodeName
Simplest Scheduling Method: nodeName is the simplest way to instruct Kubernetes to schedule a pod onto a specific node.

Direct Instruction: When you specify a nodeName in your pod's manifest, you are directly telling Kubernetes to deploy that pod to the node with the matching name.

Example Usage: For example, if you set nodeName: minikube-m02, all replicas of your deployment will be scheduled on the minikube-m02 node.

eep in mind that using nodeName bypasses the scheduler's filtering and scoring process entirely. If the specific node you named is down or out of resources, the pod won't be moved—it simply won't run


# nodeSelector
Node selectors are a way to deploy your pods onto specific nodes based on labels.

Here's how they work:

- Label Your Nodes:
You add labels to your nodes using command **```kubectl label node <node-name> <key>=<value>```**. For example, you might label the key and value of nodes with ``team=analytics``.

- Define the Selector:
In your pod or deployment manifest, you specify the nodeSelector with a key-value pair that matches the labels on your nodes.

- Match and Deploy: 
The scheduler then picks all nodes matching that label and deploys the pods onto them. you can see the example of nodeSelector [here](nodeSelector.yaml)

Compared to nodeName, this is a much more flexible approach because it allows the scheduler to choose between multiple nodes that share the same label. 

# Node Affinity
Node Affinity is a more expressive way to schedule pods compared to nodeSelector. It offers significantly more flexibility to the Kubernetes scheduler through two main enforcement levels:

1. Preferred During Scheduling (Soft Rule)
This option tells the scheduler to try to find a node that matches specified labels (e.g., a node with node-name: arm-worker).

Behavior: If a matching node is found, the pod will be scheduled there.

Fallback: If no matching node is available, the scheduler is still allowed to place the pod on any other available node.

Example: A pod might be scheduled on a random node even if a preferred node-architecture: Windows label does not exist anywhere in the cluster. However, if the preferred label node-name: minikube-m02 is present on a node, the scheduler will prioritize that specific node.

2. Required During Scheduling (Hard Rule)
This option acts similarly to nodeSelector but with the added power of affinity operators.

Behavior: It forces the scheduler to only place the pod on a node that exactly matches the specified label.

Failure State: If no such node is found, the pod will remain in an unschedulable (Pending) state until a matching node becomes available.

# Pod Affinity and Anti-Affinity
Kubernetes Pod Affinity and Anti-Affinity
Pod Affinity is a scheduling strategy used to collocate pods within the same region or availability zone. This is particularly beneficial for applications that frequently communicate, as it significantly reduces network latency.

How Pod Affinity Works
To implement this, you define a label selector (to identify the target pods) and a topology key (to define the scope, such as a node, zone, or region).

Label Matching: The Kubernetes scheduler searches for existing pods that match the defined label selector.

Topology Identification: Once found, the scheduler identifies the value of the topology key on the node where those pods are running.

Placement: The scheduler places the new pod onto a node that shares that same topology key value.

Example Scenarios
Regional Collocation: If a "To-Do UI" application is deployed in the US-East-1 region, Pod Affinity ensures the "To-Do API" is also deployed within US-East-1.

Hard Rules: You can set strict requirements (RequiredDuringScheduling) where, for example, an API pod must be deployed on the exact same host as the UI pod by using the hostname as the topology key.

## Pod Anti-Affinity
In contrast, Pod Anti-Affinity is used to keep pods away from each other. This forces the scheduler to spread replicas across different nodes, availability zones, or regions.

Failure Domain Protection: By ensuring pods are not concentrated on a single machine, you prevent a complete service outage if a specific node or rack fails.

High Availability: This is the standard practice for ensuring that your application remains reachable even during localized infrastructure issues Here an example of [podAffinity](podAffinity.yaml).

# Taints and Tolerations
Taints and Tolerations: Controlling Pod Scheduling
Taints and Tolerations are key mechanisms used to control pod scheduling on Kubernetes nodes. Taints are applied to nodes to repel certain pods, essentially making the node unschedulable unless a pod has a matching toleration.

A common use case for tainting a node is during upgrades or maintenance the Kubernetes cluster, where you want to prevent new pods from being scheduled on that node while you perform operations like draining pods or bringing the node down.

Types of Taints
There are three popular types of taints used to manage node behavior:

NoSchedule: This makes the node completely unschedulable for pods that do not have a matching toleration.

NoExecute: This is a more aggressive taint. When applied, all pods currently running on the node that do not tolerate this taint will be immediately evicted.

PreferNoSchedule: This type of taint suggests that the scheduler should avoid scheduling pods on the node if possible, but it will still schedule them there if no other suitable nodes are available. This is useful for nodes with performance issues where you prefer not to schedule pods, but it is not a strict requirement.

Tolerations as the Exception
Tolerations are applied to pods and allow them to schedule onto nodes that have matching taints. They act as an exception to the rule. Even if a node has a "NoSchedule" taint, a pod with a toleration matching that taint’s key-value pair and effect can still be scheduled on it.

For example, high priority or production critical pods might have tolerations to ensure they can run even on tainted nodes.

Implementation
To apply a taint to a node, you can use the following command:
```sh
kubectl taint node [node-name] key1=value1:NoSchedule
```
To allow a pod to be scheduled on that node, you must add a toleration to the pod's YAML definition matching the specific key, value, and effect of the taint.