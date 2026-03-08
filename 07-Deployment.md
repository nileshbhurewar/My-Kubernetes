# 🚀 Kubernetes Deployment — Complete Guide

## 📘 Overview

A **Deployment** in Kubernetes is a higher-level abstraction that manages **ReplicaSets** and ensures the desired number of Pods are running. It provides declarative updates for Pods and ReplicaSets — meaning you describe the desired state, and Kubernetes makes it happen automatically.

Deployments are one of the most commonly used Kubernetes objects for managing applications.

---

## 🎯 Why Use Deployments?

| Purpose                       | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| **Scalability**               | Easily scale applications up or down           |
| **Self-healing**              | Automatically replaces failed Pods             |
| **Rolling Updates**           | Deploy new versions of apps without downtime   |
| **Rollback**                  | Revert to previous versions in case of failure |
| **Declarative Configuration** | Define the desired state in YAML               |

---

## 🧩 Deployment vs ReplicaSet vs Pod

| Resource       | Purpose                                                   |
| -------------- | --------------------------------------------------------- |
| **Pod**        | Smallest deployable unit — runs containers                |
| **ReplicaSet** | Ensures a specific number of identical Pods are running   |
| **Deployment** | Manages ReplicaSets and provides update/rollback features |

---

## ⚙️ Creating a Deployment

### Option 1: Using `kubectl`

```bash
kubectl create deployment nginx-deploy --image=nginx --replicas=3
```

### Option 2: Using YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: nginx
spec:
  replicas: 3
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
        image: nginx:latest
        ports:
        - containerPort: 80
```

Apply it:

```bash
kubectl apply -f deployment.yaml
```

---

## 📋 Common Commands

### 🔹 List all deployments

```bash
kubectl get deployments -n nginx
```

### 🔹 Describe a deployment

```bash
kubectl describe deployment nginx-deploy -n nginx
```

### 🔹 View ReplicaSets created by a deployment

```bash
kubectl get rs -n nginx
```

### 🔹 View pods managed by a deployment

```bash
kubectl get pods -l app=nginx -n nginx
```

### 🔹 Scale deployment

```bash
kubectl scale deployment nginx-deploy --replicas=5 -n nginx
```

### 🔹 Update container image

```bash
kubectl set image deployment/nginx-deploy nginx=nginx:1.25 -n nginx
```

### 🔹 Rollback deployment

```bash
kubectl rollout undo deployment/nginx-deploy -n nginx
```

### 🔹 Check rollout status

```bash
kubectl rollout status deployment/nginx-deploy -n nginx
```

### 🔹 View rollout history

```bash
kubectl rollout history deployment/nginx-deploy -n nginx
```

---

## 🧠 Go Inside a Pod (from Deployment)

```bash
# List pods under deployment
kubectl get pods -n nginx

# Go inside one of the pods
kubectl exec -it <pod-name> -n nginx -- bash

# Example
kubectl exec -it nginx-deploy-abc123 -n nginx -- bash
curl 127.0.0.1
```

### 🔹 Describe Pod from Deployment

```bash
kubectl describe pod <pod-name> -n nginx
```

### 🔹 Check Logs of a Pod

```bash
kubectl logs <pod-name> -n nginx
```

To view logs of a specific container:

```bash
kubectl logs <pod-name> -c <container-name> -n nginx
```

Follow logs continuously:

```bash
kubectl logs -f <pod-name> -n nginx
```

---

## 🔄 Rolling Update Example

Update deployment image gradually without downtime.

```bash
kubectl set image deployment/nginx-deploy nginx=nginx:1.27 -n nginx
```

Kubernetes will:

* Create new Pods with the new image
* Wait until they’re running
* Terminate old Pods

You can check progress:

```bash
kubectl rollout status deployment/nginx-deploy -n nginx
```

If something goes wrong:

```bash
kubectl rollout undo deployment/nginx-deploy -n nginx
```

---

## 🧮 Example: Resource Limits in Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deploy
  namespace: dev
spec:
  replicas: 2
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
      - name: app
        image: nginx
        resources:
          requests:
            memory: "128Mi"
            cpu: "250m"
          limits:
            memory: "256Mi"
            cpu: "500m"
```

---

## ⚠️ Troubleshooting Commands

| Task                  | Command                                            |
| --------------------- | -------------------------------------------------- |
| Get Deployment Events | `kubectl describe deployment <name> -n <ns>`       |
| Check Pod Status      | `kubectl get pods -n <ns>`                         |
| Get Pod Logs          | `kubectl logs <pod-name> -n <ns>`                  |
| View ReplicaSets      | `kubectl get rs -n <ns>`                           |
| Check Rollout Status  | `kubectl rollout status deployment/<name> -n <ns>` |
| Delete Deployment     | `kubectl delete deployment <name> -n <ns>`         |

---

## 🧾 Summary

| Concept      | Description                                                           |
| ------------ | --------------------------------------------------------------------- |
| Purpose      | Declaratively manage Pods & ReplicaSets                               |
| Key Features | Scaling, Rolling Updates, Rollbacks                                   |
| Commands     | `kubectl get`, `kubectl describe`, `kubectl scale`, `kubectl rollout` |
| Use Case     | Reliable app deployment and version control                           |

---

## 🖼️ Visual Overview (Text)

```
+-------------------------------------------------------+
|                   Kubernetes Cluster                  |
|-------------------------------------------------------|
| Deployment: nginx-deploy                              |
|   -> ReplicaSet: nginx-deploy-12345                   |
|       -> Pod: nginx-deploy-abcde                      |
|       -> Pod: nginx-deploy-fghij                      |
|       -> Pod: nginx-deploy-klmno                      |
+-------------------------------------------------------+
```

✅ **In short:** Deployments are the most reliable way to run, scale, and update applications in Kubernetes with zero downtime and automatic rollback capability.
