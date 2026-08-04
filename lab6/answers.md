## Checkpoint Q2

Before deletion, the frontend Pod had the IP address `<old-IP>`. After I deleted and recreated the Pod from the same manifest, it received the IP address `<new-IP>`.

The IP changed because Pods are ephemeral resources. When a Pod is deleted, that specific Pod and its network identity are destroyed. Recreating the manifest creates a new Pod instance, and Kubernetes assigns it a new IP address. Therefore, applications should not depend permanently on an individual Pod IP.


#Checkpoint Q3

When one pod was deleted, Kubernetes noticed that the actual number of running pods (2) was less than the desired number (3). The Deployment controller detected this difference and automatically created a new pod to restore the desired state. This demonstrates Kubernetes' self-healing feature using the control loop: Desired State → Controller Watches → Gap Detected → Reconcile → Desired State Restored.
