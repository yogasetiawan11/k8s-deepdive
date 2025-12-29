# What is a Probe in Kubernetes (Simple Definition)

<img width="1400" height="733" alt="Image" src="https://github.com/user-attachments/assets/f1a8c808-8209-4787-a8af-b17936f0dc63" />

A probe is a way for Kubernetes to check the health of a container.

Kubernetes does not guess if your app is healthy. It asks the container using probes.

Think like this:

> “Hey container, are you alive?”
> “Are you ready to receive traffic?”
> “Have you finished starting up?”

Each question is answered by a different probe.

# Why Probes Are Needed (Need for a Probe)

## Without probes, Kubernetes has no idea if:

- Your app is stuck

- Your app is running but broken

- Your app started but not ready

- Your app needs restart

## Problems without probes

- Users get errors even though Pod is Running

- Traffic sent to broken containers

- Pods never restart when app hangs

- Slow-starting apps get killed too early

## What probes solve

- Auto-restart broken containers

- Stop traffic to unhealthy pods

- Wait until app is truly ready

- Improve availability and reliability

👉 Probes turn Kubernetes from “container runner” into “self-healing platform.”

# Types of Probes in Kubernetes

Kubernetes has 3 probe types:

- Probe Type	Purpose
- Liveness	Is the app still alive?
- Readiness	Can the app receive traffic?
- Startup	Has the app finished starting?


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
