# 📈 Kubernetes Horizontal Pod Autoscaler (HPA) – Complete Guide

---

## 1️⃣ What is Horizontal Pod Autoscaler (HPA)?

**Horizontal Pod Autoscaler (HPA)** automatically **scales the number of pod replicas** in a workload (Deployment, StatefulSet, ReplicaSet) based on **resource usage or custom metrics**.

📌 In simple words:

> When load increases → pods increase
> When load decreases → pods decrease

HPA is a **core Kubernetes feature** used heavily in production.

---

## 2️⃣ Why HPA is Needed (Real-World Reason)

In real applications:

* Traffic is not constant
* Peak load during office hours
* Sudden spikes (sales, campaigns)

### Without HPA ❌

* Manual scaling required
* Application downtime
* Over-provisioning resources

### With HPA ✅

* Automatic scaling
* Cost optimization
* High availability
* Better user experience

---

## 3️⃣ What HPA Can Scale

HPA can scale:

* Deployments
* ReplicaSets
* StatefulSets

HPA **cannot scale**:

* DaemonSets

---

## 4️⃣ How HPA Works (Internal Flow)

1. Metrics Server collects metrics
2. HPA Controller checks metrics periodically
3. Desired replicas calculated
4. Scale up/down triggered

📌 HPA runs every **15 seconds (default)**

---

## 5️⃣ Prerequisite: Metrics Server

HPA **requires Metrics Server**.

### Check Metrics Server

```bash
kubectl get deployment metrics-server -n kube-system
```

### Install (Minikube)

```bash
minikube addons enable metrics-server
```

---

## 6️⃣ HPA Metrics Types

### A. Resource Metrics (Most Common)

| Metric | Description        |
| ------ | ------------------ |
| cpu    | CPU utilization    |
| memory | Memory utilization |

---

### B. Custom Metrics

* Application-specific metrics
* Requests per second
* Queue length

---

### C. External Metrics

* Cloud metrics (ALB requests, SQS depth)
* Prometheus / CloudWatch

---

## 7️⃣ CPU-Based HPA Example

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: cpu-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: web-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

✔ Scales pods when CPU usage > 70%

---

## 8️⃣ Important: Requests are Mandatory

HPA uses **CPU requests**, NOT limits.

❌ No CPU request → HPA will not work

Example:

```yaml
resources:
  requests:
    cpu: "200m"
```

---

## 9️⃣ Memory-Based HPA Example

```yaml
metrics:
- type: Resource
  resource:
    name: memory
    target:
      type: Utilization
      averageUtilization: 75
```

---

## 🔟 How Desired Replicas are Calculated

Formula:

```
desiredReplicas = currentReplicas * ( currentMetric / targetMetric )
```

Example:

* Current replicas: 2
* CPU usage: 140%
* Target: 70%

```
2 * (140 / 70) = 4 replicas
```

---

## 1️⃣1️⃣ Scale Up vs Scale Down Behavior

* Scale Up → Fast
* Scale Down → Slow (stabilization window)

📌 Prevents frequent scaling (flapping)

---

## 1️⃣2️⃣ HPA with Custom Metrics Example

```yaml
metrics:
- type: Pods
  pods:
    metric:
      name: requests_per_second
    target:
      type: AverageValue
      averageValue: 100
```

---

## 1️⃣3️⃣ HPA Status & Debugging

### Check HPA

```bash
kubectl get hpa
```

### Describe HPA

```bash
kubectl describe hpa cpu-hpa
```

Shows:

* Current metrics
* Desired replicas
* Scaling events

---

## 1️⃣4️⃣ What Happens When HPA Cannot Get Metrics?

❌ No metrics → No scaling

Reasons:

* Metrics server down
* No CPU requests defined
* Wrong API version

---

## 1️⃣5️⃣ HPA vs VPA vs CA

| Feature   | HPA            | VPA           | Cluster Autoscaler |
| --------- | -------------- | ------------- | ------------------ |
| Scales    | Pods           | Pod resources | Nodes              |
| Direction | Horizontal     | Vertical      | Infrastructure     |
| Use case  | Traffic spikes | Right sizing  | Capacity           |

---

## 1️⃣6️⃣ HPA + ResourceQuota

⚠ Important:

HPA **cannot scale beyond ResourceQuota limits**.

Example:

* HPA maxReplicas: 20
* ResourceQuota pods: 10

➡ Max pods = 10

---

## 1️⃣7️⃣ HPA + Taints & Tolerations

* HPA increases replicas
* Scheduler must find suitable nodes
* Taints may block scheduling

📌 Always ensure enough eligible nodes

---

## 1️⃣8️⃣ Real-World Production Use Cases

### 🧠 Web Applications

* Scale on CPU / RPS

### 🧠 APIs

* Scale on latency / queue size

### 🧠 Batch Consumers

* Scale on queue depth

---

## 1️⃣9️⃣ Common Interview Questions

### Q1. Does HPA work without CPU requests?

❌ No

### Q2. Can HPA scale StatefulSets?

✅ Yes

### Q3. Can HPA scale to zero?

❌ No (needs KEDA)

### Q4. HPA checks metrics every?

✅ 15 seconds

Golden Rule (MOST IMPORTANT)

Once HPA exists, HPA controls the replica count — not the Deployment.

The replicas: 4 in deployment.yml is only the starting point, not the final authority.

---

## 2️⃣0️⃣ Production Best Practices

✅ Always define CPU requests
✅ Set realistic min/max replicas
✅ Monitor scaling behavior
✅ Combine with Cluster Autoscaler
✅ Test scaling with load

---

## 2️⃣1️⃣ Common Mistakes

❌ Missing metrics server
❌ No CPU requests
❌ Very low maxReplicas
❌ Ignoring quotas

---

## 2️⃣2️⃣ Summary

✔ HPA automatically scales pods
✔ Uses metrics server
✔ CPU-based scaling most common
✔ Essential for cloud-native apps
✔ Very important interview topic

---
