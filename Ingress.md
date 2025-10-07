# Ingress Controller Setup Documentation

This guide demonstrates how to set up an **Ingress Controller** in Kubernetes to route traffic between **Nginx** and **Apache2** applications under the same namespace.

---

## 🧩 Namespace Configuration

**File: `namespace.yml`**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: nilesh
```

This creates an isolated namespace called `nilesh` for all related Kubernetes resources.

---

## 🌐 Deployments

### **Nginx Deployment**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nilesh
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

### **Apache2 Deployment**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: apache2-deployment
  labels:
    app: apache2
  namespace: nilesh
spec:
  replicas: 1
  selector:
    matchLabels:
      app: apache2
  template:
    metadata:
      labels:
        app: apache2
    spec:
      containers:
      - name: apache2
        image: httpd:latest
        ports:
        - containerPort: 80
```

---

## ⚙️ Services

### **Apache2 Service**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: apache2-service
  namespace: nilesh
spec:
  selector:
    app: apache2
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30081
  type: NodePort
```

### **Nginx Service**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: nilesh
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30080
  type: NodePort
```

---

## 🚦 Ingress Configuration

**File: `ingress.yml`**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-traffic-router
  namespace: nilesh
  # Annotation needed to tell the Nginx Ingress Controller how to handle the path matching
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  # The Ingress Controller will use the LoadBalancer/NodePort it provides
  ingressClassName: nginx # Use the name of your specific Ingress Controller
  rules:
  # A host is not mandatory, but if you don't use one, all traffic will match this rule.
  # If you have a domain, use: host: "example.com"
  - http:
      paths:
      # Rule 1: Route traffic from /apache to the apache2-service
      - path: /apache
        # The pathType Exact means only /apache will match.
        # Prefix means /apache, /apache/foo, /apache/bar all match.
        pathType: Prefix 
        backend:
          service:
            name: apache2-service # Name of your Apache Service
            port:
              number: 80 # The target port on the Service
      # Rule 2: Route traffic from /nginx to the nginx-service
      - path: /nginx
        pathType: Prefix
        backend:
          service:
            name: nginx-service # Name of your Nginx Service
            port:
              number: 80 # The target port on the Service
```

This Ingress resource routes traffic to respective services:

* `/apache` → **Apache2 Service**
* `/nginx` → **Nginx Service**

---

## 🧭 Testing the Setup

1. Get your Minikube IP:

   ```bash
   minikube ip
   ```

   Example output:

   ```bash
   192.168.49.2
   ```

2. Edit your `/etc/hosts` file and add the following entry:

   ```bash
   192.168.49.2  web.host
   ```

3. Test the routes using `curl`:

   ```bash
   curl http://web.host/nginx
   curl http://web.host/apache
   ```

If everything is configured correctly, you should receive default pages from **Nginx** and **Apache2** respectively.

---

## ✅ Verification Commands

Check resources in the namespace:

```bash
kubectl get all -n nilesh
```

Check ingress rules:

```bash
kubectl get ingress -n nilesh
```

Describe ingress for debugging:

```bash
kubectl describe ingress web-traffic-router -n nilesh
```

---

## 🧹 Cleanup

To delete all resources created in this namespace:

```bash
kubectl delete namespace nilesh
```

---

📘 **Summary:**
This configuration sets up an isolated namespace with two web applications (**Nginx** and **Apache2**) managed by an **Nginx Ingress Controller**, allowing traffic routing via specific paths on a single IP address.

<img width="539" height="358" alt="image" src="https://github.com/user-attachments/assets/3511b927-5626-4f54-95e8-f249c2b7448d" />

