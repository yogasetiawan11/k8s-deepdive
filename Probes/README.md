# What is a Probe in Kubernetes (Simple Definition)

<img width="1400" height="733" alt="Image" src="https://github.com/user-attachments/assets/f1a8c808-8209-4787-a8af-b17936f0dc63" />

Kubernetes uses probes to determine the health and availability of a container running inside a Pod. Probes help Kubernetes decide when to restart a container, when to send traffic, or when to wait during startup.

There are three main probe mechanisms:
- Exec Probe

- HTTP Probe

- TCP Probe

Each probe has different behavior and use cases.

## Probing Mechanisms

| Probe Type | Configuration Example | Success Condition | Failure Condition |
|-----------|-----------------------|------------------|------------------|
| **Exec** | `exec:`<br>`command:`<br>`- mongo`<br>`- --eval`<br>`- "db.adminCommand('ping')"` | Exit code `0` | Exit code `1` |
| **HTTP** | `httpGet:`<br>`path: /health`<br>`port: 8080` | HTTP status `200–399` | Any status other than `200–399` |
| **TCP** | `tcpSocket:`<br>`port: 8080` | Port accepts traffic | Port cannot accept traffic |

# Why Probes Are Needed (Need for a Probe)

In Kubernetes, a container running does not always mean healthy. An application can stay alive while being unresponsive, stuck, or unable to serve traffic. Probes exist to close this gap.

Below are the core reasons probes are required, especially in production systems.

1. Containers Can Be Alive but Broken

A container may:

- Start successfully

- Keep its process running

- Still fail internally

Examples:

- Application thread deadlock

- Database connection pool exhausted

- Memory leak causing extreme latency

- Without probes, Kubernetes assumes everything is fine.

Probes give Kubernetes real application-level feedback.

2. Automatic Self-Healing

Probes enable Kubernetes to take action automatically. Depending on probe type:

- Restart the container (Liveness)

- Stop sending traffic (Readiness)

- Wait before marking container ready (Startup)

This removes the need for:

- Manual restarts

- Human intervention during failures

- Result: self-healing infrastructure.

3. Prevent Sending Traffic to Unhealthy Pods

Without readiness probes:

- Service continues routing traffic

- Users hit failing instances

With readiness probes:

- Unready pods are removed from Service endpoints

- Healthy pods continue serving traffic

This is critical for:

- Zero-downtime deployments

- High availability systems

4. Safe Application Startup Handling

Some applications:

- Need warm-up time

- Load large models

- Run database migrations

Without probes:

- Kubernetes may restart the container too early

Startup probes tell Kubernetes:

- “The app is not ready yet, but it is still starting.”

- This prevents restart loops during initialization.

5. Enable Rolling Updates Without Downtime

During rolling deployments:

- Old pods terminate

- New pods start gradually

Probes ensure:

- Traffic goes only to ready pods

- New pods receive traffic only after fully initialized

- This makes rolling updates safe and predictable.

6. Better Observability and Debugging

Probe failures provide:

- Clear signals in Pod events

- Actionable logs

- Metrics for monitoring systems

> Instead of: “The app is slow sometimes”

>You get: “Readiness probe failed due to dependency timeout”

## What probes solve

- Auto-restart broken containers

- Stop traffic to unhealthy pods

- Wait until app is truly ready

- Improve availability and reliability

👉 Probes turn Kubernetes from “container runner” into “self-healing platform.”



# Liveness Probe
Liveness Probe checks whether the application is still alive.It answers one question:

> “Is the app running correctly, or is it stuck?”
>If Kubernetes decides the app is not alive, it will restart the container.

## Why Liveness Probe is needed
In K8s environment Sometimes an application:
- Hangs
- Enters deadlock
- Stops responding but process still runs

## Without liveness probe:
1. Kubernetes thinks the Pod is fine
2. Users keep getting errors
3. App never restarts

## Liveness probe allows Kubernetes to:
- Detect a broken app
- Restart the container automatically
- Recover without human action
- When to use Liveness Probe

# Readiness Probe
Readiness Probe checks whether the application is ready to receive traffic. It answers this question:

> “Can this app handle requests right now?”
If readiness probe fails:
- Traffic is stopped
- Container is not restarted

## Why Readiness Probe is needed
An app may be running but:
- Still starting
- Waiting for database
- Overloaded
- Temporarily unhealthy

## Without readiness probe:
- Traffic goes to unready pods
- Users get errors
- Deployments cause downtime
- Readiness probe allows Kubernetes to:
- Send traffic only to ready pods
- Remove unhealthy pods from Service
- Achieve zero-downtime deployments

# Startup Probe
Startup Probe checks whether the application has finished starting. It answers this question:

> “Has the app started successfully?”
While startup probe is running:
- Liveness probe is disabled
- Readiness probe is disabled

## Why Startup Probe is needed
Some apps start very slowly like:
- Java applications
- Apps with migrations
- Machine learning models
- Legacy systems

## Without startup probe:
- Liveness probe runs too early
- App gets restarted again and again
- Pod enters CrashLoopBackOff

# Startup probe tells Kubernetes:

> “Wait, I’m still starting”

> “Don’t restart me yet”
