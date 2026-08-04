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

## Checkpoint Q7

The frontend and API tiers use Deployments because they are stateless. Their pods do not need permanent identities or dedicated persistent storage, so Kubernetes can replace or scale them freely.

PostgreSQL uses a StatefulSet because it is stateful. The StatefulSet gives the database pod a stable name, postgres-0, predictable startup ordering, and persistent storage through a PersistentVolumeClaim. Even when the pod is replaced, it can reconnect to the same stored database data.


## Checkpoint Q8

No, the data would normally not survive if PostgreSQL were deployed as a plain Deployment without a PersistentVolumeClaim. Data stored only inside a container belongs to that container's temporary writable filesystem. When the pod is deleted and replaced, the new pod receives a new container filesystem. The PersistentVolumeClaim stores the PostgreSQL data separately from the pod, allowing the recreated postgres-0 pod to reconnect to the same data.


## Checkpoint Q9

The broken pod showed ErrImagePull and later ImagePullBackOff. This does not exactly match the lecture's listed statuses of Running, Pending, CrashLoopBackOff, or OOMKilled. It is a related container waiting status.

ErrImagePull means Kubernetes failed to download the specified container image. ImagePullBackOff means Kubernetes continues retrying the image download but waits for increasing periods between attempts. In this case, the failure occurred because nginx:definitely-not-a-real-tag does not exist.
