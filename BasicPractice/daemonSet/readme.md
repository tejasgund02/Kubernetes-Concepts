# Kubernetes DaemonSet

## Overview
A **DaemonSet** ensures that a copy of a Pod runs on all (or some) nodes in a Kubernetes cluster. It is useful for running background services such as logs collection, monitoring agents, and networking components on each node.

## Use Cases
- Running a cluster-wide logging agent (e.g., Fluentd, Logstash)
- Deploying monitoring agents (e.g., Prometheus Node Exporter, Datadog Agent)
- Managing networking components (e.g., CNI plugins, kube-proxy)
- Running security tools (e.g., Falco, AppArmor profiles)

## How DaemonSet Works
- When a new node is added to the cluster, a Pod is automatically scheduled on it.
- When a node is removed, the DaemonSet Pod on that node is also removed.
- Updates to a DaemonSet apply changes to all running Pods managed by it.

## Creating a DaemonSet
Below is an example YAML manifest for a simple DaemonSet:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
  labels:
    app: example
spec:
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: example-container
        image: busybox
        command: ["sh", "-c", "while true; do echo Hello from DaemonSet; sleep 10; done"]
```

## Managing DaemonSets
### Creating a DaemonSet
```sh
kubectl apply -f daemonset.yaml
```

### Listing DaemonSets
```sh
kubectl get daemonsets
```

### Describing a DaemonSet
```sh
kubectl describe daemonset example-daemonset
```

### Deleting a DaemonSet
```sh
kubectl delete daemonset example-daemonset
```

## Node Selector & Tolerations
DaemonSets can be scheduled only on specific nodes using:
- **Node Selectors**: Restrict Pods to certain nodes.
- **Tolerations & Taints**: Allow scheduling on tainted nodes.

Example:
```yaml
spec:
  template:
    spec:
      nodeSelector:
        disktype: ssd
      tolerations:
      - key: "example-key"
        operator: "Exists"
        effect: "NoSchedule"
```

## Rolling Updates for DaemonSets
DaemonSets can be updated using **RollingUpdate** strategy:
```yaml
spec:
  updateStrategy:
    type: RollingUpdate
```
Apply changes using:
```sh
kubectl apply -f daemonset.yaml
```

## Conclusion
DaemonSets are a powerful way to ensure that critical Pods run on every node in a Kubernetes cluster. They are essential for logging, monitoring, networking, and security tasks.

## References
- [Kubernetes Documentation - DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

