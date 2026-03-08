# 04 - kubectl (Kubernetes Command Line Tool)

## 1. Introduction

kubectl is the **command-line interface (CLI)** used to interact with a Kubernetes cluster.

It allows users to:

* Deploy applications
* Inspect cluster resources
* View logs
* Debug issues
* Manage cluster configurations

kubectl communicates with the **kube-apiserver** of the Kubernetes control plane.

---

# 2. How kubectl Works

kubectl does not directly communicate with worker nodes.

All commands go through the **API Server**.

Flow:

User → kubectl → API Server → Kubernetes Components → Cluster

Example:

kubectl get pods

The request is sent to the API server which returns pod information.

---

# 3. Installing kubectl (Linux)

Example installation:

curl -LO "[https://dl.k8s.io/release/$(curl](https://dl.k8s.io/release/$%28curl) -L -s [https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/stable.txt%29/bin/linux/amd64/kubectl)"

chmod +x kubectl

sudo mv kubectl /usr/local/bin/

Verify installation:

kubectl version --client

---

# 4. kubeconfig File

kubectl uses a configuration file called **kubeconfig**.

Location:

~/.kube/config

This file contains:

* Cluster information
* Authentication details
* Context configuration

Example kubeconfig structure:

clusters:
users:
contexts:
current-context:

---

# 5. Contexts in Kubernetes

A **context** defines which cluster kubectl is communicating with.

This is useful when managing multiple clusters.

View contexts:

kubectl config get-contexts

Switch context:

kubectl config use-context <context-name>

---

# 6. Namespaces

Namespaces are used to logically separate resources within a cluster.

Default namespaces:

* default
* kube-system
* kube-public
* kube-node-lease

View namespaces:

kubectl get namespaces

Create namespace:

kubectl create namespace dev

Run command in namespace:

kubectl get pods -n dev

---

# 7. Basic kubectl Commands

View cluster nodes

kubectl get nodes

View pods

kubectl get pods

View pods with more details

kubectl get pods -o wide

View all resources

kubectl get all

Describe resource

kubectl describe pod <pod-name>

Delete resource

kubectl delete pod <pod-name>

---

# 8. Imperative vs Declarative Commands

## Imperative Approach

Commands executed directly.

Example:

kubectl run nginx --image=nginx

## Declarative Approach

Using YAML configuration files.

Example:

kubectl apply -f deployment.yaml

Declarative approach is preferred in production.

---

# 9. Creating Resources

Create resource from YAML file:

kubectl apply -f pod.yaml

Create deployment:

kubectl create deployment nginx --image=nginx

Scale deployment:

kubectl scale deployment nginx --replicas=3

---

# 10. Updating Resources

Edit resource:

kubectl edit deployment nginx

Apply updated YAML:

kubectl apply -f deployment.yaml

---

# 11. Viewing Logs

Check container logs:

kubectl logs <pod-name>

Check logs of specific container:

kubectl logs <pod-name> -c <container-name>

Stream logs:

kubectl logs -f <pod-name>

---

# 12. Executing Commands in Pods

Execute command inside container:

kubectl exec -it <pod-name> -- /bin/bash

Example:

kubectl exec -it nginx-pod -- /bin/bash

---

# 13. Port Forwarding

Access pod locally:

kubectl port-forward pod/nginx-pod 8080:80

Now application is accessible on:

[http://localhost:8080](http://localhost:8080)

---

# 14. Copy Files to Pods

Copy file from local system to pod:

kubectl cp file.txt nginx-pod:/tmp/file.txt

Copy file from pod to local:

kubectl cp nginx-pod:/tmp/file.txt ./file.txt

---

# 15. Debugging Commands

Check pod status:

kubectl get pods

Detailed pod information:

kubectl describe pod <pod-name>

View cluster events:

kubectl get events

Check node information:

kubectl describe node <node-name>

---

# 16. Resource Monitoring

View resource usage:

kubectl top nodes

kubectl top pods

Note:

Metrics Server must be installed for these commands.

---

# 17. Label Management

Labels are key-value pairs attached to resources.

Add label:

kubectl label pod nginx-pod env=dev

View labels:

kubectl get pods --show-labels

---

# 18. Export Resource YAML

View YAML configuration of resource:

kubectl get pod nginx-pod -o yaml

Save YAML to file:

kubectl get deployment nginx -o yaml > deployment.yaml

---

# 19. Common kubectl Shortcuts

Use aliases for faster commands.

Example:

alias k=kubectl

Then run:

k get pods

---

# 20. Important Interview Questions

### What is kubectl?

kubectl is the command-line tool used to interact with Kubernetes clusters and manage resources.

### How does kubectl communicate with the cluster?

kubectl communicates with the **kube-apiserver** using REST API calls.

### Where is kubectl configuration stored?

In the **kubeconfig file located at ~/.kube/config**.

### What is the difference between apply and create?

kubectl create creates a new resource.

kubectl apply updates an existing resource or creates it if it doesn't exist.

---

# 21. Summary

kubectl is the primary tool used to interact with Kubernetes clusters.

Key capabilities:

* Deploy applications
* Manage resources
* Debug issues
* Monitor cluster state

Mastering kubectl commands is essential for Kubernetes administrators and DevOps engineers.
