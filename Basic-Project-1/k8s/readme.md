# 🚀 Kubernetes Ingress Guide (Using Kind)

## 📌 What is Kubernetes Ingress?
Kubernetes **Ingress** is used to expose services running inside a cluster to the external world using HTTP(S) routing. It provides a way to manage access to multiple services using a single external IP, reducing the need for multiple LoadBalancers or NodePorts.

## ✅ Why Use Ingress?
- **Exposes multiple services** through a single IP using domain-based routing.
- **Supports path-based routing** (`/app1` → Service1, `/app2` → Service2).
- **Handles SSL/TLS termination** for secure HTTPS connections.
- **Acts as a Layer 7 (HTTP/HTTPS) load balancer**.
- **Reduces cloud costs** by minimizing LoadBalancers.

---

## 🏗 How to Set Up Ingress in Kubernetes (Using Kind)

### **1️⃣ Create a Kind Cluster with Ingress Support**
Create a `kind-config.yaml` file with the following content:
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 80  # Map HTTP
        hostPort: 80
      - containerPort: 443  # Map HTTPS
        hostPort: 443
```
Now create the cluster:
```bash
kind create cluster --config kind-config.yaml
```

---

### **2️⃣ Deploy an NGINX Ingress Controller**
Apply the official NGINX Ingress controller manifest:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

Wait for the Ingress controller to be ready:
```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

---

### **3️⃣ Create an Ingress Resource**
Create a file `my-ingress.yaml` with the following content:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.local
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: service-app1
            port:
              number: 80
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: service-app2
            port:
              number: 80
```

---

### **4️⃣ Apply the Ingress Resource**
```bash
kubectl apply -f my-ingress.yaml
```

---

### **5️⃣ Update /etc/hosts for Local Testing**
Since Kind runs inside Docker, map the domain `myapp.local` to `127.0.0.1`:
```bash
echo "127.0.0.1 myapp.local" | sudo tee -a /etc/hosts
```

---

### **6️⃣ Verify Ingress is Working**
Check ingress status:
```bash
kubectl get ingress
```
Test in the browser or use `curl`:
```bash
curl http://myapp.local/app1
curl http://myapp.local/app2
```

---

## 🌟 Common Ingress Controllers
- **NGINX Ingress Controller** (most widely used)
- **Traefik** (lightweight and dynamic configuration)
- **HAProxy Ingress** (high performance)
- **Contour** (designed for Envoy proxy)

---

## 🏆 When to Use Ingress?
✅ When you need **domain-based routing** for multiple services.
✅ When you want to **terminate SSL/TLS** at the Kubernetes level.
✅ When you need a **cost-effective alternative** to LoadBalancers.

---

## 🚀 Summary
- Ingress simplifies access to multiple services inside Kubernetes.
- Requires an **Ingress Controller** to manage routing.
- Supports **path-based and host-based routing**.
- Reduces dependency on multiple LoadBalancers or NodePorts.

---

## 💡 Contribution
Feel free to **contribute** to this guide by submitting a pull request! 🚀

---

## 📜 License
This document is **licensed under the MIT License**.
