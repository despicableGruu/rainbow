# Kubernetes Rainbow Deploys

## Overview

This repository demonstrates a technique for deploying Kubernetes services that require extended draining periods for established connections. It utilizes a "rainbow" approach, leveraging multiple deployments and service selectors to achieve graceful transitions without disrupting ongoing interactions.

## Prerequisites

- Kubernetes cluster (e.g., Minikube)
- Docker
- Make

## Demo

The repository includes a simple Go application serving HTTP and TCP connections.

1. **Initialize:**
   - Start Minikube.
   - Configure Docker environment.
   - Build the Docker image (`make image`).
   - Deploy the application (`make install`).

2. **Observe:**
   - Verify deployment and service creation using `kubectl get deployments` and `kubectl get services`.
   - Access the HTTP service via the exposed port (e.g., through `minikube service list`).
   - Establish a TCP connection using `telnet`.

3. **Deploy New Version:**
   - Modify the Git repository and commit changes.
   - Rebuild the Docker image (`make image`).
   - Deploy the new version (`make install`).

4. **Observe Draining:**
   - Note that new requests are routed to the updated deployment.
   - Existing TCP connections remain on the old deployment.
   - Open a new TCP connection to see the new version's behavior.
   - Delete the older deployment (`kubectl delete deployment <older-deployment>`).

5. **Success!**

## Implementation

The Kubernetes configuration uses a Service selector that targets Deployments based on a color label. This label is generated from the Git commit hash, ensuring uniqueness for each deployment.  The `make install` process dynamically updates the label, leading to a new deployment on every update. Cleanup of old deployments is left to the user.