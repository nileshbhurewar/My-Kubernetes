# 08 - DaemonSets

## 1. Introduction

A **DaemonSet** is a Kubernetes controller that ensures a copy of a specific pod runs on **every node in the cluster**.

Whenever a new node is added to the cluster, the DaemonSet automatically schedules a pod on that node.

If a node is removed, the pod running on that node is also removed.

DaemonSets are commonly used for **node-level services**.

Examples include:

* Log collection agents
* Monitoring agents
* Network plugins
* Storage daemons

---

# 2. Why DaemonSets Are Needed

Some applications must run on **every node** instead of a few selected nodes.

Examples:

1. Log collectors (Fluentd, Filebeat)
2. Monitoring agents (Prometheus Node Exporter)
3. Network plugins (Calico, Cilium)
4. Security monitoring tools

Without DaemonSets, administrators would have to manually schedule pods on every node.

DaemonSets automate this process.

---

# 3. How DaemonSets Work

DaemonSets automatically ensure that **one pod instance runs on each node**.

Example cluster:

Node-1
Node-2
Node-3

If a DaemonSet is deployed:

Node-1 → Pod
Node-2 → Pod
Node-3 → Pod

If Node-4 joins the cluster:

Node-4 → Pod (automatically created)

---

# 4. DaemonSet Architecture

Structure:

DaemonSet
↓
Ensures pod runs on every node

Each node runs exactly **one DaemonSet pod** unless configured otherwise.

---

# 5. DaemonSet YAML Structure

Example configuration:

apiVersion: apps/v1
kind: DaemonSet
metadata:
name: fluentd-daemonset
spec:
selector:
matchLabels:
app: fluentd
template:
metadata:
labels:
app: fluentd
spec:
containers:
- name: fluentd
image: fluent/fluentd
resources:
limits:
memory: 200Mi
cpu: 200m

Important fields:

selector → identifies pods managed by the DaemonSet

template → defines the pod specification

---

# 6. Creating a DaemonSet

Apply the YAML file:

kubectl apply -f daemonset.yaml

Check DaemonSets:

kubectl get daemonsets

Check pods created by DaemonSet:

kubectl get pods

You will see one pod per node.

---

# 7. Viewing DaemonSet Details

Describe DaemonSet:

kubectl describe daemonset fluentd-daemonset

Check pods with node information:

kubectl get pods -o wide

This shows which node each pod is running on.

---

# 8. Updating DaemonSets

DaemonSets support rolling updates.

Example updating container image:

kubectl set image daemonset/fluentd-daemonset fluentd=fluent/fluentd:v2

Kubernetes will gradually update pods on each node.

---

# 9. Deleting DaemonSets

Delete DaemonSet:

kubectl delete daemonset fluentd-daemonset

This will delete all pods created by the DaemonSet.

---

# 10. Node Selectors

Sometimes you may want DaemonSet pods to run only on specific nodes.

This can be done using **nodeSelector**.

Example:

spec:
template:
spec:
nodeSelector:
disktype: ssd

This schedules pods only on nodes with that label.

---

# 11. Taints and Tolerations

DaemonSets can also run on **tainted nodes** such as control plane nodes.

To allow this, you add tolerations.

Example:

spec:
template:
spec:
tolerations:
- key: node-role.kubernetes.io/control-plane
operator: Exists
effect: NoSchedule

This allows the DaemonSet pod to run on control plane nodes.

---

# 12. Real World Example

Example: Log collection using Fluentd.

Every node produces logs.

A Fluentd DaemonSet runs on every node and collects logs.

Flow:

Application Pods → Node Logs → Fluentd DaemonSet → Log Storage

This ensures logs from all nodes are collected automatically.

---

# 13. DaemonSet vs Deployment

Deployment

* Runs a specified number of replicas
* Pods may run on any nodes

DaemonSet

* Runs one pod per node
* Ensures coverage across the entire cluster

Example:

Deployment → 3 pods

DaemonSet → 1 pod per node

---

# 14. Debugging DaemonSets

Check DaemonSets:

kubectl get daemonsets

Check pods:

kubectl get pods

Describe DaemonSet:

kubectl describe daemonset <name>

Check events:

kubectl get events

---

# 15. Best Practices

Recommended practices:

* Use DaemonSets for node-level services
* Use resource limits to avoid node overload
* Combine with node selectors if needed

DaemonSets are essential for cluster-wide services.

---

# 16. Important Interview Questions

### What is a DaemonSet in Kubernetes?

A DaemonSet is a controller that ensures a pod runs on every node in the Kubernetes cluster.

### What are common use cases of DaemonSets?

Common use cases include log collection, monitoring agents, networking plugins, and security tools.

### What happens when a new node is added to the cluster?

The DaemonSet automatically creates a pod on the new node.

---

# 17. Summary

DaemonSets ensure that a pod runs on every node in a Kubernetes cluster.

Key features:

* One pod per node
* Automatically handles new nodes
* Used for cluster-wide services

DaemonSets are widely used for monitoring, logging, and networking components in Kubernetes clusters.
