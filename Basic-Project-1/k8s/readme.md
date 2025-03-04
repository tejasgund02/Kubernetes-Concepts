# 🚀 Kubernetes Ingress Guide

## 📌 What is Kubernetes Ingress?
Kubernetes **Ingress** is used to expose services running inside a cluster to the external world using HTTP(S) routing. It provides a way to manage access to multiple services using a single external IP, reducing the need for multiple LoadBalancers or NodePorts.

## ✅ Why Use Ingress?
- **Exposes multiple services** through a single IP using domain-based routing.
- **Supports path-based routing** (`/app1` → Service1, `/app2` → Service2).
- **Handles SSL/TLS termination** for secure HTTPS connections.
- **Acts as a Layer 7 (HTTP/HTTPS) load balancer**.
- **Reduces cloud costs** by minimizing LoadBalancers.

---

## 🏗 How to Set Up Ingress in Kubernetes

### **1️⃣ Deploy an Ingress Controller**
For **Minikube**, enable the built-in NGINX Ingress controller:
```bash
minikube addons enable ingress
```
For **cloud-managed Kubernetes clusters (EKS, AKS, GKE)**, install an NGINX Ingress controller:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

---

### **2️⃣ Create an Ingress Resource**
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
  - host: myapp.example.com
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

### **3️⃣ Apply the Ingress Resource**
```bash
kubectl apply -f my-ingress.yaml
```

---

### **4️⃣ Update DNS or /etc/hosts (For Local Testing)**
If running locally, map the domain (`myapp.example.com`) in `/etc/hosts`:
```bash
echo "$(minikube ip) myapp.example.com" | sudo tee -a /etc/hosts
```

---

### **5️⃣ Verify Ingress is Working**
Check ingress status:
```bash
kubectl get ingress
```
Test in the browser or use `curl`:
```bash
curl http://myapp.example.com/app1
curl http://myapp.example.com/app2
```

---

## 🌟 Common Ingress Controllers
- **NGINX Ingress Controller** (most widely used)
- **Traefik** (lightweight and dynamic configuration)
- **HAProxy Ingress** (high performance)
- **Contour** (designed for Envoy proxy)
- **AWS ALB Ingress** (AWS-specific solution)

---

## 🏆 When to Use Ingress?
✅ When you need **domain-based routing** for multiple services.
✅ When you want to **terminate SSL/TLS** at the Kubernetes level.
✅ When you need a **cost-effective alternative** to cloud LoadBalancers.

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
