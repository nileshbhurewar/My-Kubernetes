# ⚖️ Kubernetes: Deployment vs StatefulSet vs ReplicaSet — Complete Guide

## 📘 Overview

In Kubernetes, **Deployments**, **StatefulSets**, **ReplicaSets**, and **ReplicationControllers** are key controllers that manage how Pods are created, scaled, and maintained. Each serves a specific purpose in managing workloads.

---

## 🧩 1. What is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes that encapsulates one or more containers with shared storage, network, and specifications on how to run the containers.

---

## 🔁 2. What is a Replica?

A **Replica** is simply an instance (copy) of a Pod. Replicas ensure **high availability** — multiple copies of the same Pod run to handle failures and load balancing.

You define the number of replicas using the `replicas:` field in Deployment, ReplicaSet, or StatefulSet.

Example:

```yaml
spec:
  replicas: 3
```

This means Kubernetes maintains **3 running Pods** at all times.

---

## ⚙️ 3. What is a ReplicationController?

A **ReplicationController (RC)** is the **old mechanism** in Kubernetes used to ensure a specified number of Pod replicas are running.

### 🔹 Limitations

* It doesn’t support **label selectors** as flexibly as ReplicaSets.
* It’s now **deprecated** and replaced by **ReplicaSet**.

### Example:

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 2
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## 🧮 4. What is a ReplicaSet?

A **ReplicaSet (RS)** ensures a specified number of Pod replicas are running at any given time — it is the **modern replacement** for ReplicationController.

### 🔹 Key Features

* Supports **set-based label selectors**.
* Automatically **recreates Pods** if they fail or are deleted.
* Used **internally by Deployments** to manage Pod lifecycles.

### Example:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
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
        image: nginx
```

### 🔹 Common Commands

```bash
kubectl get rs -n nginx
kubectl describe rs nginx-rs -n nginx
kubectl scale rs nginx-rs --replicas=5 -n nginx
```

---

## 🧱 5. What is a Deployment?

A **Deployment** provides **declarative updates** for Pods and ReplicaSets. It automates creation, scaling, updates, and rollback of Pods.

### 🔹 Key Features

* Manages **ReplicaSets** automatically.
* Supports **rolling updates** and **rollbacks**.
* Ideal for **stateless applications**.

### Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
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
        image: nginx
```

---

## 🧩 6. What is a StatefulSet?

A **StatefulSet** is used to manage **stateful applications** that require stable, persistent storage and consistent network identities.

### 🔹 Key Features

| Feature                          | Description                                                 |
| -------------------------------- | ----------------------------------------------------------- |
| **Stable Network ID**            | Each Pod gets a unique hostname (e.g., web-0, web-1, web-2) |
| **Ordered Deployment & Scaling** | Pods start and stop in a specific order                     |
| **Persistent Storage**           | Each Pod has its own PersistentVolumeClaim                  |
| **Stable Pod Identity**          | Pods retain identity even if rescheduled                    |

### Example:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-db
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-persistent-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

---

## 🔍 7. Key Differences

| Feature                | Deployment             | StatefulSet                | ReplicaSet            | ReplicationController |
| ---------------------- | ---------------------- | -------------------------- | --------------------- | --------------------- |
| **Purpose**            | Stateless applications | Stateful applications      | Maintain Pod replicas | Legacy controller     |
| **Pod Identity**       | Random                 | Stable & Ordered           | Random                | Random                |
| **Storage**            | Shared/ephemeral       | Dedicated PersistentVolume | Shared/ephemeral      | Shared/ephemeral      |
| **Pod Name**           | Random hash suffix     | Fixed (e.g., web-0, web-1) | Random hash           | Random hash           |
| **Rolling Updates**    | Yes                    | Ordered updates            | Manual                | Manual                |
| **Rollback**           | Yes                    | Manual                     | No                    | No                    |
| **Used Internally By** | —                      | —                          | Deployment            | —                     |
| **Persistence**        | No                     | Yes                        | No                    | No                    |

---

## 🧠 8. When to Use What?

| Use Case                                | Recommended Controller    |
| --------------------------------------- | ------------------------- |
| Stateless web app (e.g., Nginx, API)    | **Deployment**            |
| Database, Kafka, Redis, Cassandra       | **StatefulSet**           |
| Custom scaling control without rollouts | **ReplicaSet**            |
| Legacy/older Kubernetes cluster         | **ReplicationController** |

---

## 🧾 9. Common Commands

```bash
# Get all Deployments, StatefulSets, and ReplicaSets
kubectl get deploy,sts,rs -A

# Describe specific resource
a. kubectl describe deployment nginx-deploy -n nginx
b. kubectl describe statefulset mysql-db -n database
c. kubectl describe rs nginx-rs -n nginx

# Scale resources
kubectl scale deployment nginx-deploy --replicas=5 -n nginx
kubectl scale statefulset mysql-db --replicas=4 -n database

# Check rollout status
kubectl rollout status deployment/nginx-deploy -n nginx
kubectl rollout status statefulset/mysql-db -n database
```

---

## 🧮 10. Example Architecture (Text View)

```
+------------------------------------------------------------+
|                  Kubernetes Cluster                        |
|------------------------------------------------------------|
| Deployment (nginx)                                         |
|   -> ReplicaSet (nginx-rs-12345)                           |
|       -> Pods: nginx-abc, nginx-def, nginx-ghi              |
|                                                            |
| StatefulSet (mysql-db)                                     |
|   -> Pods: mysql-db-0, mysql-db-1, mysql-db-2               |
|   -> PersistentVolumeClaims: pvc-mysql-db-0,...             |
|                                                            |
| ReplicaSet (custom-app-rs)                                 |
|   -> Pods: custom-app-1, custom-app-2, custom-app-3         |
|                                                            |
+------------------------------------------------------------+
```

---

## ✅ 11. Summary Table

| Concept                   | Purpose                    | Persistent | Update Type | Rollback | Pod Identity | Common Use                 |
| ------------------------- | -------------------------- | ---------- | ----------- | -------- | ------------ | -------------------------- |
| **Deployment**            | Manage stateless workloads | ❌          | Rolling     | ✅        | Random       | Web servers, APIs          |
| **StatefulSet**           | Manage stateful workloads  | ✅          | Ordered     | Manual   | Stable       | Databases, Kafka           |
| **ReplicaSet**            | Maintain replica count     | ❌          | Manual      | ❌        | Random       | Internal use by Deployment |
| **ReplicationController** | Legacy replication control | ❌          | Manual      | ❌        | Random       | Deprecated                 |

---

✅ **In short:**

* **Deployment** → Ideal for stateless apps with scaling and updates.
* **StatefulSet** → Ideal for stateful apps needing persistent identity.
* **ReplicaSet** → Ensures replica count; used by Deployments.
* **ReplicationController** → Legacy, replaced by ReplicaSet.
