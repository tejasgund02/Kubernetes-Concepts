# Kubernetes PersistentVolume (PV) & PersistentVolumeClaim (PVC)

## Overview
**PersistentVolumes (PVs)** and **PersistentVolumeClaims (PVCs)** in Kubernetes provide a way to manage storage resources that persist beyond the lifecycle of individual pods.

- **PersistentVolume (PV):** Represents a physical storage resource available in the cluster.
- **PersistentVolumeClaim (PVC):** A request for storage by a user or application.

## Use Cases
- Storing database files that persist beyond pod restarts.
- Sharing storage between multiple pods.
- Retaining logs, backups, or other critical application data.

## PersistentVolume (PV)
A **PersistentVolume** is a cluster-wide resource that defines a specific storage unit.

### Example PV YAML
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: "/mnt/data"
```

## PersistentVolumeClaim (PVC)
A **PersistentVolumeClaim** is a request for storage by a pod.

### Example PVC YAML
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: manual
```

## Using PVC in a Pod
Once a PVC is bound to a PV, it can be used in a Pod.

### Example Pod Using PVC
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: example-container
    image: nginx
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: storage
  volumes:
  - name: storage
    persistentVolumeClaim:
      claimName: example-pvc
```

## Managing PV and PVC
### Create PV & PVC
```sh
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
```

### List PVs and PVCs
```sh
kubectl get pv
kubectl get pvc
```

### Describe PV and PVC
```sh
kubectl describe pv example-pv
kubectl describe pvc example-pvc
```

### Delete PV & PVC
```sh
kubectl delete pvc example-pvc
kubectl delete pv example-pv
```

## PersistentVolume Reclaim Policies
- **Retain:** Keeps the PV after PVC is deleted.
- **Recycle:** Cleans the PV and makes it available for reuse.
- **Delete:** Automatically removes the PV when PVC is deleted.

## Conclusion
- PVs provide storage resources that persist across pod restarts.
- PVCs allow applications to request and use storage in a decoupled manner.
- Proper reclaim policies help manage storage lifecycle effectively.

## References
- [Kubernetes Documentation - PersistentVolumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

