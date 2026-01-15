# there are several attack surfaces for Kubernetes pods and how to mitigate them:

## **Running as Root**:
By default, containers run as the root user, which grants the highest privileges in the operating system. If a pod is compromised while running as root, an attacker can perform actions like installing utilities (e.g., curl), downloading malware (e.g., crypto miners), and setting up backdoor SSH access, potentially without detection.
Even if a container doesn't run as root, if tools like curl are already present in the base Docker image, an attacker can still use them to carry out malicious activities .
## **Privilege Escalation**:
Linux systems allow users to temporarily elevate their privileges (e.g., using sudo) to perform administrative functions. If this capability is not restricted in a container, an attacker could exploit it to gain more control.
## **Remote Code Execution (RCE) Vulnerabilities**:
These vulnerabilities allow an attacker to execute malicious code within the pod. Even with other security measures, an RCE can enable an attacker to install malicious software, create backdoors, or manipulate the file system.
## **Writable File System**:
If a container has a writable root file system, an attacker who gains access can write malicious files, install tools, set up crypto miners, and create backdoors.
## **Linux Capabilities**:
Linux operating systems have broad capabilities, including setting machine time or interacting with hardware. If non-essential capabilities are not dropped, attackers can exploit these to stage attacks from within the container.

# To mitigate the attack surfaces, you can implement the following strategies:

## Run Containers as Non-Root Users:

- **Kubernetes securityContext**: Configure the pod's securityContext to set runAsUser to a non-root user ID. This forces all containers in the pod to run as non-root.
- **Docker File**: Alternatively, install a non-privileged user directly within your Dockerfile and switch to that user using the USER instruction. This allows for local testing of the non-root functionality before deployment.
- **Enforce Non-Root Execution**: Add the runAsNonRoot: true setting to your pod's securityContext. This enforces that either runAsUser is set or a non-root user is configured in the Dockerfile.


## Reduce Image Size and Unnecessary Tools:

- **Use Smaller Base Images**: Switch from larger base images like Debian or Ubuntu to smaller alternatives like Debian Bookworm slim or Alpine Linux. This significantly reduces the number of pre-installed tools and binaries, thereby shrinking the attack surface.
- **Use scratch for Static Binaries**: If your application compiles to a static binary (e.g., Go or Rust), consider using a scratch base image. This creates an extremely minimal image containing only your application binary, eliminating almost all system tools and shells.

## Prevent Privilege Escalation:

- **Disable allowPrivilegeEscalation**:
Set allowPrivilegeEscalation: false within the container's securityContext. This prevents any process within the container from gaining more privileges than its parent process.
Implement Read-Only Root File System:

- **Set readOnlyRootFilesystem**: Introduce readOnlyRootFilesystem: true in your pod's securityContext. This prevents an attacker from writing malicious files, installing tools, or setting up backdoors on the file system, as the container will only be able to write to explicitly mounted volumes.
Drop Non-Essential Linux Capabilities:

Drop All Capabilities: Under the capabilities section within the container's securityContext, use drop: ["ALL"]. This removes all non-essential Linux capabilities, limiting what an attacker can do even if they gain access to the container

# Example to Implement the concept
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secure-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: secure-app
  template:
    metadata:
      labels:
        app: secure-app
    spec:
      # --- POD LEVEL SECURITY ---
      # Applies to ALL containers in the pod
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
      containers:
      - name: main-container
        image: nginx
        # --- CONTAINER LEVEL SECURITY ---
        # Applies ONLY to this specific container
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop:
              - ALL
```

Set non root user at pod level

```Dockerfile
FROM node:20-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

```


Key Points to Remember
- Precedence:
 If you define a setting (like runAsUser) at both the Pod level and the Container level, the Container level takes precedence.

- Immutability:
 Once a Pod is created by the Deployment, you cannot change the securityContext without triggering a rollout (restarting the Pods).

- Best Practice:
  Always try to use runAsNonRoot: true at the Pod level to ensure a baseline of security for every container in that deployment.
