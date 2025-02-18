# Kubernetes Deployment

## Overview
A **Deployment** in Kubernetes provides declarative updates for Pods and ReplicaSets. It ensures that the desired state of an application is maintained, enabling rolling updates and rollbacks.

## Use Cases
- Managing stateless applications.
- Performing rolling updates and rollbacks.
- Scaling applications up or down.
- Ensuring high availability and fault tolerance.

## How Deployments Work
- A Deployment manages ReplicaSets, ensuring a specified number of replicas run at all times.
- It provides a mechanism for rolling updates, reducing downtime during application upgrades.
- Supports rollbacks to a previous stable version if an update fails.

## Creating a Deployment
Below is an example YAML manifest for a simple Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
  labels:
    app: example
spec:
  replicas: 3
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
        image: nginx:latest
        ports:
        - containerPort: 80
```

## Managing Deployments
### Creating a Deployment
```sh
kubectl apply -f deployment.yaml
```

### Listing Deployments
```sh
kubectl get deployments
```

### Describing a Deployment
```sh
kubectl describe deployment example-deployment
```

### Scaling a Deployment
```sh
kubectl scale --replicas=5 deployment example-deployment
```

### Rolling Update
Modify the image version in the Deployment YAML and apply the changes:
```sh
kubectl apply -f deployment.yaml
```
Monitor the rollout:
```sh
kubectl rollout status deployment example-deployment
```

### Rolling Back a Deployment
```sh
kubectl rollout undo deployment example-deployment
```

### Deleting a Deployment
```sh
kubectl delete deployment example-deployment
```

## Deployment Strategies
- **RollingUpdate (default):** Gradually replaces old pods with new ones.
- **Recreate:** Deletes all old pods before creating new ones.

## Conclusion
- Deployments manage applications efficiently with scaling and rolling updates.
- They provide rollback capabilities for failed updates.
- Essential for maintaining high availability in Kubernetes environments.

## References
- [Kubernetes Documentation - Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)