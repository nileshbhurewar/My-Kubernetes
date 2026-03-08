# ⚙️ Kubernetes: DaemonSet vs StatefulSet — Complete Guide

## 📘 Overview

In Kubernetes, **DaemonSets** and **StatefulSets** are both workload controllers — but they serve **very different purposes**. Understanding their behavior is crucial when managing system-level services versus stateful applications.

---

## 🧩 1. What is a DaemonSet?

A **DaemonSet** ensures that a specific Pod runs **on all (or selected) nodes** in the cluster. It’s used for deploying system or monitoring agents that need to exist on every node.

### 🔹 Key Features

| Feature                   | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| **Node-level Deployment** | Ensures one Pod per node                                    |
| **Automatic Scheduling**  | New nodes automatically get a DaemonSet Pod                 |
| **Use Case**              | Background services (e.g., logging, monitoring, networking) |
| **No Persistent Storage** | DaemonSets are usually stateless                            |

### 🔹 Common Examples

* `kube-proxy` (manages networking rules on each node)
* `fluentd` or `filebeat` (log collection agents)
* `datadog-agent`, `prometheus-node-exporter` (monitoring agents)

### 🔹 Example YAML

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-monitor
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app: node-monitor
  template:
    metadata:
      labels:
        app: node-monitor
    spec:
      containers:
      - name: monitor
        image: datadog/agent:latest
        resources:
          limits:
            cpu: "100m"
            memory: "128Mi"
```

### 🔹 Commands

```bash
# List DaemonSets
kubectl get ds -A

# Describe DaemonSet
a. kubectl describe ds node-monitor -n kube-system

# Check pods created by DaemonSet
kubectl get pods -o wide -n kube-system

# Delete DaemonSet
kubectl delete ds node-monitor -n kube-system
```

---

## 🧱 2. What is a StatefulSet?

A **StatefulSet** is used for **stateful applications** that require persistent storage, ordered startup, and stable network identity.

### 🔹 Key Features

| Feature                 | Description                                              |
| ----------------------- | -------------------------------------------------------- |
| **Ordered Deployment**  | Pods are created and terminated in a specific order      |
| **Stable Pod Identity** | Pods have unique, predictable names (e.g., web-0, web-1) |
| **Persistent Storage**  | Each Pod gets its own PersistentVolumeClaim              |
| **Use Case**            | Databases and clustered applications                     |

### 🔹 Example YAML

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-db
  namespace: database
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
        - name: mysql-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-storage
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

### 🔹 Commands

```bash
# Get StatefulSets
kubectl get sts -A

# Describe StatefulSet
kubectl describe sts mysql-db -n database

# Scale StatefulSet
kubectl scale sts mysql-db --replicas=4 -n database

# Check rollout status
kubectl rollout status sts/mysql-db -n database
```

---

## 🔍 3. Key Differences

| Feature              | DaemonSet                  | StatefulSet                  |
| -------------------- | -------------------------- | ---------------------------- |
| **Purpose**          | Run a Pod on each node     | Manage stateful applications |
| **Pod Count**        | Equal to node count        | Defined replicas             |
| **Storage**          | Usually none               | Persistent for each Pod      |
| **Pod Name**         | Random                     | Stable (e.g., app-0, app-1)  |
| **Network Identity** | No fixed hostname          | Stable network ID            |
| **Update Type**      | Rolling                    | Ordered, controlled updates  |
| **Use Case**         | System/monitoring agents   | Databases, message queues    |
| **Scheduling**       | Runs on all/specific nodes | Follows StatefulSet spec     |

---

## 🧠 4. When to Use What?

| Scenario                                              | Recommended Controller |
| ----------------------------------------------------- | ---------------------- |
| Deploy monitoring/logging agents on all nodes         | **DaemonSet**          |
| Run one Pod per node for infrastructure               | **DaemonSet**          |
| Deploy a stateful database like MySQL, MongoDB, Kafka | **StatefulSet**        |
| Need persistent identity and storage per Pod          | **StatefulSet**        |

---

## 🧾 5. Common Commands Summary

```bash
# DaemonSet
kubectl get ds -A
kubectl describe ds <daemonset-name> -n <namespace>

# StatefulSet
kubectl get sts -A
kubectl describe sts <statefulset-name> -n <namespace>

# Scale StatefulSet
kubectl scale sts <name> --replicas=3 -n <namespace>

# View Pods created by controllers
kubectl get pods -o wide -n <namespace>
```

---

## 🧮 6. Example Architecture (Text View)

```
+------------------------------------------------------------+
|                    Kubernetes Cluster                      |
|------------------------------------------------------------|
| DaemonSet: node-monitor                                    |
|   -> Pod: monitor-node1 (runs on node1)                    |
|   -> Pod: monitor-node2 (runs on node2)                    |
|   -> Pod: monitor-node3 (runs on node3)                    |
|                                                            |
| StatefulSet: mysql-db                                      |
|   -> Pod: mysql-db-0 (with PVC mysql-db-0)                 |
|   -> Pod: mysql-db-1 (with PVC mysql-db-1)                 |
|   -> Pod: mysql-db-2 (with PVC mysql-db-2)                 |
+------------------------------------------------------------+
```

---

## ✅ 7. Summary Table

| Concept         | Purpose                              | Persistent | Pod Count  | Identity | Scheduling | Typical Use                       |
| --------------- | ------------------------------------ | ---------- | ---------- | -------- | ---------- | --------------------------------- |
| **DaemonSet**   | Run system-level agents on each node | ❌          | 1 per node | Random   | All nodes  | Log collectors, Monitoring agents |
| **StatefulSet** | Run stateful workloads with identity | ✅          | Custom     | Stable   | Controlled | Databases, Kafka, Redis           |

---

✅ **In short:**

* Use **DaemonSet** when you need **one Pod per node** for system services.
* Use **StatefulSet** when you need **stable identity, persistent storage, and ordered deployment** for **stateful workloads**.
