## Checkpoint Q2

Before deletion, the frontend Pod had the IP address `<old-IP>`. After I deleted and recreated the Pod from the same manifest, it received the IP address `<new-IP>`.

The IP changed because Pods are ephemeral resources. When a Pod is deleted, that specific Pod and its network identity are destroyed. Recreating the manifest creates a new Pod instance, and Kubernetes assigns it a new IP address. Therefore, applications should not depend permanently on an individual Pod IP.
