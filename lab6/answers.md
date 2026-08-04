## Checkpoint Q2

Before deletion, the frontend Pod had the IP address `<old-IP>`. After I deleted and recreated the Pod from the same manifest, it received the IP address `<new-IP>`.

The IP changed because Pods are ephemeral resources. When a Pod is deleted, that specific Pod and its network identity are destroyed. Recreating the manifest creates a new Pod instance, and Kubernetes assigns it a new IP address. Therefore, applications should not depend permanently on an individual Pod IP.


## Checkpoint Q3

When one pod was deleted, Kubernetes noticed that the actual number of running pods (2) was less than the desired number (3). The Deployment controller detected this difference and automatically created a new pod to restore the desired state. This demonstrates Kubernetes' self-healing feature using the control loop: Desired State → Controller Watches → Gap Detected → Reconcile → Desired State Restored.


## Checkpoint Q4

The frontend is managed by its own Deployment, while the database is managed separately. Kubernetes allows each service to scale independently, so increasing or decreasing the number of frontend pods does not affect the database. This allows each application tier to be managed based on its own workload and resource requirements.

## Checkpoint Q5

Port-forward provides temporary access directly to a specific pod. If that pod is deleted or replaced, the connection is lost. A Service provides a stable IP address and DNS name that automatically routes traffic to the available pods. Since pods are ephemeral and their IP addresses can change, Services ensure users can continue accessing the application without knowing the current pod IP addresses.

## Checkpoint Q6

Kubernetes performs rolling updates gradually by replacing old pods with new pods while keeping the application available. It also stores the previous Deployment revision, so the update can be rolled back using a single command if there is a problem. With Docker Compose alone, this process would be harder because containers would usually need to be stopped, recreated, and checked manually. Docker Compose does not provide the same built-in rolling update, health-controlled replacement, and automatic rollback features.
