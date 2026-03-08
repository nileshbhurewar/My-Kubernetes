# 25 - Troubleshooting

## 1. Introduction

Troubleshooting is a critical skill for Kubernetes administrators and DevOps engineers.

In real production environments, issues may occur in:

* Pods
* Nodes
* Networking
* Storage
* Applications

Effective troubleshooting helps quickly identify and resolve these problems.

---

# 2. Common Kubernetes Issues

Some common problems in Kubernetes clusters include:

Pod CrashLoopBackOff

ImagePullBackOff

Pods stuck in Pending state

Node NotReady

Service not reachable

Network connectivity issues

Understanding how to debug these issues is essential.

---

# 3. Basic Troubleshooting Approach

A structured troubleshooting approach is recommended.

Step 1

Check cluster health.

Step 2

Check node status.

Step 3

Check pod status.

Step 4

Inspect logs.

Step 5

Check events.

This systematic approach helps identify root causes faster.

---

# 4. Checking Cluster Status

Check nodes in the cluster:

kubectl get nodes

Expected output:

Nodes should show status **Ready**.

If nodes show **NotReady**, there may be network or kubelet issues.

---

# 5. Checking Pods

View pods:

kubectl get pods

Detailed pod information:

kubectl get pods -o wide

This shows:

* Pod IP
* Node location
* Status

---

# 6. Describing Resources

The describe command provides detailed information about resources.

Example:

kubectl describe pod <pod-name>

This command shows:

* Events
* Container status
* Error messages

It is one of the most useful debugging commands.

---

# 7. Checking Logs

Logs help diagnose application issues.

View logs:

kubectl logs <pod-name>

View logs of specific container:

kubectl logs <pod-name> -c <container-name>

Stream logs:

kubectl logs -f <pod-name>

Logs help identify runtime errors.

---

# 8. Executing Commands Inside Pods

You can access a running container to debug issues.

Example:

kubectl exec -it <pod-name> -- /bin/bash

Inside the container you can check:

* Running processes
* Application files
* Network connectivity

---

# 9. Checking Events

Cluster events provide useful debugging information.

Example command:

kubectl get events

Events show:

* Scheduling issues
* Image pull errors
* Resource failures

---

# 10. Debugging CrashLoopBackOff

CrashLoopBackOff occurs when containers repeatedly crash.

Common causes:

Application errors

Missing configuration

Incorrect environment variables

Debug steps:

Check logs:

kubectl logs <pod-name>

Describe pod:

kubectl describe pod <pod-name>

---

# 11. Debugging ImagePullBackOff

ImagePullBackOff occurs when Kubernetes cannot pull container images.

Possible causes:

Incorrect image name

Private registry authentication failure

Network issues

Example fix:

Verify image name and registry credentials.

---

# 12. Debugging Pending Pods

Pods remain in **Pending state** when they cannot be scheduled.

Possible reasons:

Insufficient CPU or memory

Node selector mismatch

Taints and tolerations

Check events to identify scheduling problems.

---

# 13. Node Troubleshooting

Check node details:

kubectl describe node <node-name>

Check node resource usage:

kubectl top nodes

Common node issues:

High CPU usage

Memory pressure

Disk pressure

---

# 14. Networking Troubleshooting

Check services:

kubectl get svc

Check endpoints:

kubectl get endpoints

Test DNS resolution:

kubectl exec -it <pod-name> -- nslookup my-service

These commands help diagnose networking issues.

---

# 15. Storage Troubleshooting

Check Persistent Volumes:

kubectl get pv

Check Persistent Volume Claims:

kubectl get pvc

Describe PVC:

kubectl describe pvc <pvc-name>

Storage problems usually involve volume binding issues.

---

# 16. Useful Troubleshooting Commands

Important commands:

kubectl get pods

kubectl describe pod <pod>

kubectl logs <pod>

kubectl exec -it <pod> -- /bin/bash

kubectl get events

kubectl get nodes

kubectl get svc

These commands are used frequently during debugging.

---

# 17. Real World Troubleshooting Example

Scenario:

Application not accessible.

Troubleshooting steps:

1. Check pod status
2. Check service configuration
3. Verify endpoints
4. Inspect logs

Root cause:

Service selector mismatch with pod labels.

Fix:

Update service selector.

---

# 18. Troubleshooting Best Practices

Recommended practices:

Follow systematic debugging steps

Use logs and events to identify issues

Monitor resource usage

Keep Kubernetes components updated

Troubleshooting skills improve with real-world experience.

---

# 19. Important Interview Questions

### What is the first step when troubleshooting Kubernetes issues?

Check the status of nodes and pods using kubectl commands.

### Which command shows detailed information about a pod?

kubectl describe pod <pod-name>

### How do you check container logs?

kubectl logs <pod-name>

### How do you access a running container?

kubectl exec -it <pod-name> -- /bin/bash

---

# 20. Summary

Troubleshooting is essential for maintaining reliable Kubernetes clusters.

Key steps:

Check cluster health

Inspect pod status

Analyze logs and events

Investigate networking and storage

Strong troubleshooting skills help DevOps engineers resolve issues quickly and maintain production stability.
