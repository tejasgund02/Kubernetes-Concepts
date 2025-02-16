# Kubectl Useful Commands Cheat Sheet

## 1. Cluster Information
```bash
kubectl cluster-info          # Display cluster information
kubectl get nodes             # List all nodes in the cluster
kubectl version               # Show client and server versions
```

---

## 2. Working with Namespaces
```bash
kubectl get namespaces        # List all namespaces
kubectl create namespace my-namespace  # Create a new namespace
kubectl delete namespace my-namespace  # Delete a namespace
```

---

## 3. Managing Pods
```bash
kubectl get pods                        # List all pods in the default namespace
kubectl get pods -n my-namespace        # List pods in a specific namespace
kubectl describe pod my-pod             # Show details of a specific pod
kubectl logs my-pod                     # View logs of a pod
kubectl exec -it my-pod -- /bin/sh      # Access a running pod
kubectl delete pod my-pod               # Delete a pod
```

---

## 4. Deployments
```bash
kubectl get deployments                   # List all deployments
kubectl describe deployment my-deployment # Show details of a deployment
kubectl delete deployment my-deployment   # Delete a deployment
kubectl scale deployment my-deployment --replicas=3  # Scale a deployment
# example : kubectl scale deployment/nginx-deployment -n nginx --replicas=5
kubectl rollout restart deployment my-deployment     # Restart a deployment
```

---

## 5. Services & Networking
```bash
kubectl get services                      # List all services
kubectl describe service my-service       # Show details of a service
kubectl delete service my-service         # Delete a service
kubectl port-forward pod/my-pod 8080:80   # Forward a local port to a pod
```

---

## 6. ConfigMaps & Secrets
```bash
kubectl get configmaps                    # List all ConfigMaps
kubectl create configmap my-config --from-literal=key1=value1  # Create a ConfigMap
kubectl describe configmap my-config      # Show details of a ConfigMap
kubectl delete configmap my-config        # Delete a ConfigMap

kubectl get secrets                       # List all secrets
kubectl create secret generic my-secret --from-literal=password=12345  # Create a Secret
kubectl describe secret my-secret         # Show details of a Secret
kubectl delete secret my-secret           # Delete a Secret
```

---

## 7. Persistent Volumes & Storage
```bash
kubectl get pv                            # List Persistent Volumes
kubectl get pvc                           # List Persistent Volume Claims
kubectl describe pvc my-pvc               # Show details of a PVC
kubectl delete pvc my-pvc                 # Delete a PVC
```

---

## 8. Monitoring & Debugging
```bash
kubectl top pods                          # Show resource usage of pods
kubectl top nodes                         # Show resource usage of nodes
kubectl events                            # Show cluster events
```

---

## 9. Apply & Delete Resources from YAML
```bash
kubectl apply -f my-deployment.yaml       # Apply configuration from a YAML file
kubectl delete -f my-deployment.yaml      # Delete resources from a YAML file
```

---

## 10. Miscellaneous
```bash
kubectl config view                       # View Kubernetes config file
kubectl config use-context my-cluster     # Switch Kubernetes context
kubectl get all                           # List all resources in the default namespace
kubectl explain pod                       # Show documentation for a resource
```

---

## 11. Uninstalling Resources
```bash
kubectl delete all --all                  # Delete all resources in the current namespace
kubectl delete namespace my-namespace     # Delete a namespace and all its resources
```

---

## 12. Additional Resources
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

This cheat sheet provides commonly used `kubectl` commands for managing Kubernetes clusters. 🚀
