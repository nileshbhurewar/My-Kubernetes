# 📦 Kubernetes Resource Quotas – Complete Guide

---

## 1️⃣ What is ResourceQuota in Kubernetes?

**ResourceQuota** is a Kubernetes object used to **limit and control resource consumption** within a **namespace**.

It prevents a single team, application, or user from **over-consuming cluster resources** like:

* CPU
* Memory
* Pods
* Services
* PersistentVolumeClaims
* LoadBalancers

📌 ResourceQuota works **at namespace level**, not per pod or node.

---

## 2️⃣ Why Resource Quotas are Needed (Real‑World Reason)

In real production clusters:

* Multiple teams share the same cluster
* One team can accidentally deploy too many pods
* One app can consume all CPU/memory

### Without ResourceQuota ❌

* Cluster becomes unstable
* Other applications starve
* Nodes crash due to OOM (Out Of Memory)

### With ResourceQuota ✅

* Fair resource distribution
* Cost control
* Predictable performance
* Better multi‑tenant isolation

---

## 3️⃣ Types of Resource Quotas

### A. Compute Resource Quotas

Controls **CPU and Memory usage**

| Resource        | Description            |
| --------------- | ---------------------- |
| requests.cpu    | Total CPU requested    |
| limits.cpu      | Max CPU limit          |
| requests.memory | Total memory requested |
| limits.memory   | Max memory limit       |

---

### B. Object Count Quotas

Controls **number of Kubernetes objects**

| Resource               | Description           |
| ---------------------- | --------------------- |
| pods                   | Number of pods        |
| services               | Services count        |
| services.loadbalancers | LoadBalancer services |
| configmaps             | ConfigMaps            |
| secrets                | Secrets               |
| persistentvolumeclaims | PVCs                  |
| replicationcontrollers | RCs                   |
| resourcequotas         | Quotas                |

---

### C. Storage Resource Quotas

Controls **storage usage**

| Resource                                                     | Description       |
| ------------------------------------------------------------ | ----------------- |
| requests.storage                                             | Total PVC storage |
| persistentvolumeclaims                                       | PVC count         |
| <storage-class>.storageclass.storage.k8s.io/requests.storage | Storage per class |

---

### D. Extended / Custom Resource Quotas

For extended resources like:

* GPUs
* Custom device plugins

Example:

```
requests.nvidia.com/gpu: 4
```

---

## 4️⃣ Basic ResourceQuota Example

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: basic-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "6"
    limits.memory: 12Gi
```

🔹 This means:

* Max 10 pods
* CPU requests ≤ 4 cores
* Memory requests ≤ 8Gi
* CPU limits ≤ 6 cores
* Memory limits ≤ 12Gi

---

## 5️⃣ ResourceQuota with Object Limits Example

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota
  namespace: test
spec:
  hard:
    pods: "5"
    services: "3"
    configmaps: "10"
    secrets: "10"
```

---

## 6️⃣ ResourceQuota for Storage

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: storage-quota
  namespace: prod
spec:
  hard:
    persistentvolumeclaims: "5"
    requests.storage: 100Gi
```

---

## 7️⃣ Storage Class Based Quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: gold-storage-quota
  namespace: prod
spec:
  hard:
    gold.storageclass.storage.k8s.io/requests.storage: 50Gi
```

✔ Limits only PVCs using **gold** storage class

---

## 8️⃣ How ResourceQuota Works Internally

1. Namespace is created
2. ResourceQuota applied
3. User tries to create pod/service
4. Kubernetes API Server checks:

   * Requested resources
   * Existing usage
   * Quota limits
5. If limit exceeded → **Request rejected**

---

## 9️⃣ Viewing ResourceQuota

### Check quotas

```
kubectl get resourcequota -n dev
```

### Describe quota

```
kubectl describe resourcequota basic-quota -n dev
```

Output shows:

* Used resources
* Hard limits

---

## 🔟 What Happens When Quota is Exceeded?

### Example

If pod requests 2 CPU but quota remaining is only 1 CPU:

❌ Pod creation fails

```
Error from server (Forbidden): exceeded quota
```

---

## 1️⃣1️⃣ ResourceQuota vs LimitRange

| Feature    | ResourceQuota        | LimitRange                  |
| ---------- | -------------------- | --------------------------- |
| Scope      | Namespace            | Namespace                   |
| Purpose    | Total resource limit | Default/min/max per pod     |
| Controls   | Overall usage        | Individual container limits |
| Mandatory? | No                   | No                          |

📌 Best practice: **Use both together**

---

## 1️⃣2️⃣ ResourceQuota + LimitRange (Best Practice)

* ResourceQuota → Controls total usage
* LimitRange → Enforces defaults

### Without LimitRange ❌

Pods may not define requests/limits

### With LimitRange ✅

Pods automatically get default limits

---

## 1️⃣3️⃣ Multiple ResourceQuotas in One Namespace

✔ Allowed

Kubernetes **adds all quotas together** and enforces all

Example:

* quota‑1 limits pods
* quota‑2 limits CPU

Both apply simultaneously

---

## 1️⃣4️⃣ PriorityClass & ResourceQuota

You can apply quota **per priority class**

Example:

```yaml
spec:
  hard:
    pods: "5"
  scopeSelector:
    matchExpressions:
    - scopeName: PriorityClass
      operator: In
      values: [high-priority]
```

---

## 1️⃣5️⃣ Scopes in ResourceQuota

| Scope          | Meaning                         |
| -------------- | ------------------------------- |
| BestEffort     | Pods without requests/limits    |
| NotBestEffort  | Pods with requests/limits       |
| Terminating    | Pods with activeDeadlineSeconds |
| NotTerminating | Pods without deadline           |
| PriorityClass  | Based on priority               |

---

## 1️⃣6️⃣ Common Interview Questions

### Q1. Is ResourceQuota applied per node?

❌ No, per namespace

### Q2. Can ResourceQuota limit CPU per pod?

❌ No, use LimitRange

### Q3. Can ResourceQuota limit storage?

✅ Yes

### Q4. Can we exceed quota?

❌ No, API server blocks it

### Q5. Is ResourceQuota mandatory?

❌ Optional but recommended

---

## 1️⃣7️⃣ Production Best Practices

✅ Always apply ResourceQuota in shared clusters
✅ Combine with LimitRange
✅ Separate namespaces per team
✅ Monitor quota usage
✅ Set realistic limits

---

## 1️⃣8️⃣ Real‑World Use Case

### Scenario

Company with 3 teams:

* Dev
* QA
* Prod

Each team gets:

* Fixed CPU
* Fixed memory
* Limited pods

✔ No team affects others
✔ Stable cluster

---

## 1️⃣9️⃣ Summary

✔ ResourceQuota limits total resource usage
✔ Works at namespace level
✔ Prevents resource starvation
✔ Critical for multi‑tenant clusters
✔ Used heavily in production & interviews

---


