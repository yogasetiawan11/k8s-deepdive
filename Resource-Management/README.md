# **Resource Management**
resource management in Kubernetes is a core concept for running stable, efficient, and predictable clusters.

Before started First we have to know two main types of computing resources CPU and Memory:

## **CPU (Central Processing Unit):**
 Think of the CPU as the "brain" of the computer. It does all the calculations and runs the programs.
It's measured in "milli-cores" or "CPUs". For example, 200 milli-cores is a small fraction of a CPU, and 1 CPU is like having a whole brain at your disposal.
CPU is a "compressible" resource. This means if an application tries to use too much CPU, the system will slow it down or "throttle" it, rather than shutting it down completely .
## **Memory:**
 This is like the computer's "short-term memory". It's where the computer temporarily stores the data that the CPU is actively using.
Memory is measured in bytes, like kilobytes (KB), megabytes (MB), or gigabytes (GB).
Unlike CPU, memory is "not compressible". If an application tries to use more memory than it's allowed, the system won't just slow it down; it will actually "kill" or shut down that application. This is why managing memory is very important!


# There are 4 types practice in Resource Management
Here's a breakdown of what "requests" are in Kubernetes:

## **Request**

- Guaranteed Resources:
 Requests are a way to tell Kubernetes the minimum amount of CPU and memory an application needs to start and run properly.
- Scheduling Decision:
 When you define a request, Kubernetes will only deploy your application onto a node (a computer in the cluster) that has at least that much CPU and memory available.
- Pending State:
 If no node has the requested resources, your application will stay in a "pending" state and won't start until those resources become available on a node.
- No Request Means No Guarantee:
 If you don't specify requests for CPU or memory, Kubernetes assumes you don't care how much CPU time your application gets. In the worst case, it might not get any CPU time at all, potentially affecting its performance .

In the simple term When we shcedule the pods kubernetes will define the pods on to each Node on the cluster where the minimum capacity is available and automatically assign the pods to Node. 

## **Limit**
"limits" in Kubernetes refer to the maximum CPU and memory resources a container or pod is allowed to consume. This helps prevent a single application from over-utilizing node resources and impacting other applications.


- Purpose of Limits:
 Limits are used to restrict the maximum CPU and memory that a container can use. This is crucial for preventing resource exhaustion and ensuring the stability of other applications on the same node.
- CPU vs. Memory Limits:
 If a pod exceeds its CPU limit, the CPU usage is throttled, meaning its performance will slow down but the process won't be killed.
  If a pod exceeds its memory limit, the process will be killed (Out Of Memory Killed) and restarted, as memory is not a compressible resource.
- Defining Limits :
 Limits are defined within the resources section of a container's manifest, similar to requests.

## **QoS**
Quality of Service (QoS) in Kubernetes as a mechanism to determine which pods should be killed if a node runs out of memory. Kubernetes automatically assigns a QoS class to a pod based on how resources (CPU and memory) are requested and limited in its definition

Kubenetes decides which pods will kill by categorizing the pods into 3 classess:
- **Best Effort:**
Assigned when no requests or limits are defined for the pod.
These pods are the first to be killed when a node experiences memory pressure, as Kubernetes assumes they are not critical.
- **Guaranteed:**
Assigned when requests and limits are defined and are equal for all containers within the pod.
These pods are guaranteed to receive their requested resources and are the last to be killed during resource contention.
- **Burstable:**
Assigned when requests and limits are defined, but they are not equal, or when only requests are defined.
These pods are killed after Best Effort pods but before Guaranteed pods in a resource-constrained scenario.


## **Limit Range**
 LimitRange as a Kubernetes resource that allows administrators to control resource usage at the namespace level. It enables setting minimum, maximum, and default resource values for pods and containers within a specific namespace.

Here's how LimitRange works:

- Defining Constraints:
 You can specify minimum and maximum CPU and memory limits for pods and containers.
 For example, the video demonstrates setting a minimum CPU of 50 millicores and a minimum memory of 5 MB. If a pod requests less than the minimum or more than the maximum defined in the LimitRange, the pod creation will be rejected.

- Setting Defaults:
 If a pod definition does not specify requests or limits, the LimitRange can automatically apply default values.

- Limit-Request Ratio:
 You can also define a maximum ratio between the CPU/memory limit and request for a container, ensuring that limits are not disproportionately higher than requests.
- Persistent Volume Claim Limits:
 LimitRange can also be used to define storage claim limits for Persistent Volume Claims (PVCs)