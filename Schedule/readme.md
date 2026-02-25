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