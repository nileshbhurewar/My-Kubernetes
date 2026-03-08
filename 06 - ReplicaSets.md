# 06 - ReplicaSets

## 1. Introduction

A **ReplicaSet** is a Kubernetes controller that ensures a specified number of pod replicas are running at any given time.

It helps maintain application availability by automatically creating or deleting pods to match the desired replica count.

ReplicaSets are mainly used by **Deployments**, and in most real-world scenarios you manage ReplicaSets through Deployments.

---

# 2. Why ReplicaSets Are Needed

Pods are not reliable when running alone.

If a pod crashes or a node fails, the pod disappears and the application becomes unavailable.

ReplicaSets solve this problem by ensuring:

* A fixed number of pods are always running
* New pods are automatically created if existing pods fail

Example:

If replicas = 3

Running pods:

Pod-1
Pod-2
Pod-3

If Pod-2 crashes, ReplicaSet automatically creates a new pod.

---

# 3. ReplicaSet Architecture

Structure:

ReplicaSet
↓
Manages multiple Pods

ReplicaSet continuously monitors pod status and ensures the desired number of replicas is maintained.

---

# 4. ReplicaSet YAML Structure

Example ReplicaSet configuration:

apiVersion: apps/v1
kind: ReplicaSet
metadata:
name: nginx-replicaset
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
ports:
- containerPort: 80

Important fields:

replicas → number of pods to run

selector → identifies pods managed by ReplicaSet

template → defines pod specification

---

# 5. Creating a ReplicaSet

Apply YAML file:

kubectl apply -f replicaset.yaml

Check ReplicaSet:

kubectl get rs

Check pods:

kubectl get pods

---

# 6. Scaling ReplicaSets

Increase replicas:

kubectl scale replicaset nginx-replicaset --replicas=5

Decrease replicas:

kubectl scale replicaset nginx-replicaset --replicas=2

ReplicaSet will automatically create or delete pods.

---

# 7. Viewing ReplicaSet Details

Describe ReplicaSet:

kubectl describe rs nginx-replicaset

Check pods created by ReplicaSet:

kubectl get pods -l app=nginx

---

# 8. Label Selectors

ReplicaSets use **labels** to identify which pods they manage.

Example label:

app: nginx

Selector example:

selector:
matchLabels:
app: nginx

If a pod matches this label, it is managed by the ReplicaSet.

---

# 9. Self-Healing Feature

ReplicaSets provide Kubernetes **self-healing capability**.

Example scenario:

ReplicaSet replicas = 3

Pods running:

pod-1
pod-2
pod-3

If pod-2 crashes:

ReplicaSet immediately creates a new pod.

New pods:

pod-1
pod-3
pod-4

This ensures application availability.

---

# 10. Updating ReplicaSets

ReplicaSets are not designed for application updates.

If you update the container image, existing pods will not automatically update.

This is why **Deployments** are used in production instead of directly managing ReplicaSets.

---

# 11. Deleting ReplicaSets

Delete ReplicaSet:

kubectl delete rs nginx-replicaset

Deleting the ReplicaSet also deletes the pods it manages.

---

# 12. Difference Between Pod and ReplicaSet

Pod

* Runs a single instance
* No automatic recovery

ReplicaSet

* Maintains multiple pods
* Automatically replaces failed pods

---

# 13. ReplicaSet vs Deployment

ReplicaSet

* Maintains number of pods
* No rolling updates

Deployment

* Manages ReplicaSets
* Supports rolling updates
* Supports rollback

In production, Deployments are preferred.

---

# 14. Debugging ReplicaSets

Check ReplicaSets:

kubectl get rs

Describe ReplicaSet:

kubectl describe rs <replicaset-name>

Check pods:

kubectl get pods

View events:

kubectl get events

---

# 15. Best Practices

Recommended practices:

* Use Deployments instead of standalone ReplicaSets
* Use labels carefully
* Monitor pod health

ReplicaSets are typically managed automatically by Deployments.

---

# 16. Important Interview Questions

### What is a ReplicaSet?

A ReplicaSet is a Kubernetes controller that ensures a specified number of pod replicas are always running.

### What happens if a pod managed by a ReplicaSet fails?

ReplicaSet automatically creates a new pod to maintain the desired number of replicas.

### What is the difference between ReplicaSet and Deployment?

ReplicaSet maintains the number of pods, while Deployment manages ReplicaSets and supports rolling updates and rollbacks.

---

# 17. Summary

ReplicaSets ensure high availability by maintaining the desired number of pods.

Key points:

* Maintains pod replicas
* Provides self-healing
* Uses label selectors
* Usually managed by Deployments
