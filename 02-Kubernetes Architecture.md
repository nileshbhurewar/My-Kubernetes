# 02 - Kubernetes Architecture

## 1. Overview

Kubernetes architecture explains how different components of a Kubernetes cluster work together to manage containerized applications.

A Kubernetes cluster is divided into two main parts:

1. Control Plane (Master Components)
2. Worker Nodes

The **Control Plane** manages the cluster and maintains the desired state.

The **Worker Nodes** run the actual application containers.

---

# 2. High Level Architecture Flow

Typical request flow in Kubernetes:

User → kubectl → API Server → Scheduler → Worker Node → Pod → Container

Important point:

All communication in Kubernetes goes through the **API Server**.

---

# 3. Control Plane Components

The Control Plane manages the entire cluster.

Main components:

* kube-apiserver
* etcd
* kube-scheduler
* kube-controller-manager
* cloud-controller-manager (optional)

These components ensure that the cluster always matches the **desired state**.

---

# 4. kube-apiserver

The **API Server** is the most important component of Kubernetes.

It acts as the **front-end of the Kubernetes control plane**.

All requests from:

* kubectl
* UI dashboards
* automation tools

are handled by the API Server.

## Responsibilities

* Validates requests
* Processes REST API calls
* Updates cluster state
* Communicates with etcd

Example:

When you run:

kubectl apply -f deployment.yaml

The request goes to the **API Server**, which then stores the configuration in **etcd**.

---

# 5. etcd

**etcd** is a distributed key-value database used by Kubernetes.

It stores the **entire cluster state**.

## What etcd Stores

* Pod configurations
* Node information
* Secrets
* ConfigMaps
* Deployment details
* Cluster networking configuration

## Important Points

* etcd must always be backed up
* If etcd data is lost, the cluster state is lost
* etcd is highly consistent and distributed

---

# 6. kube-scheduler

The **Scheduler** decides which node should run a newly created pod.

When a pod is created it first goes into **Pending state**.

The scheduler evaluates available nodes and selects the best one.

## Scheduling Factors

Scheduler considers:

* CPU availability
* Memory availability
* Node labels
* Affinity rules
* Taints and tolerations
* Resource requests

After selecting a node, the scheduler assigns the pod to that node.

---

# 7. kube-controller-manager

The **Controller Manager** runs background controllers that continuously monitor the cluster.

These controllers ensure that the cluster matches the desired state.

## Important Controllers

### Node Controller

Monitors node health.

If a node fails, pods are rescheduled to another node.

### ReplicaSet Controller

Ensures the correct number of pods are running.

Example:

If replicas = 3 and one pod crashes, a new pod is automatically created.

### Deployment Controller

Handles:

* Rolling updates
* Rollbacks

### Job Controller

Manages batch jobs.

---

# 8. Cloud Controller Manager

Used when Kubernetes runs in cloud environments such as:

* AWS
* Azure
* Google Cloud

## Responsibilities

* Creating cloud load balancers
* Managing cloud storage
* Node lifecycle management

---

# 9. Worker Node Components

Worker nodes run the actual application workloads.

Each worker node contains:

* kubelet
* kube-proxy
* container runtime

---

# 10. kubelet

The **kubelet** is an agent that runs on every worker node.

It communicates with the API Server.

## Responsibilities

* Receives pod specifications
* Ensures containers are running
* Starts containers
* Reports node status

### Pod Creation Flow on Node

1. Scheduler assigns pod to node
2. kubelet receives instructions
3. kubelet instructs container runtime
4. Container starts

---

# 11. kube-proxy

kube-proxy manages networking on worker nodes.

It maintains network rules that allow communication between pods and services.

## Responsibilities

* Pod to pod communication
* Service load balancing
* Network routing

kube-proxy uses:

* iptables
* IPVS

---

# 12. Container Runtime

The container runtime runs containers on worker nodes.

Examples:

* containerd
* CRI-O
* Docker (deprecated in newer Kubernetes versions)

## Responsibilities

* Pull container images
* Start containers
* Stop containers

---

# 13. Complete Pod Creation Workflow

Step 1

User creates deployment using kubectl.

Step 2

Request goes to API Server.

Step 3

Configuration is stored in etcd.

Step 4

Scheduler selects a worker node.

Step 5

kubelet receives pod specification.

Step 6

Container runtime pulls the image.

Step 7

Container starts and pod becomes running.

---

# 14. Architecture Logical Diagram

User
↓
kubectl
↓
API Server
↓
etcd
↓
Scheduler
↓
Worker Node
↓
kubelet
↓
Container Runtime
↓
Pod

---

# 15. Real World Example

Example deployment of a Spring Boot application.

1. Developer pushes Docker image to registry.
2. Deployment YAML is applied.
3. API server receives request.
4. Scheduler selects worker node.
5. kubelet starts pod.
6. Service exposes the application.

Kubernetes automatically manages scaling and failures.

---

# 16. Interview Explanation

Short answer for interviews:

"Kubernetes architecture consists of a control plane and worker nodes. The control plane manages the cluster using components like API server, scheduler, controller manager, and etcd. Worker nodes run application containers using kubelet, kube-proxy, and a container runtime."

---

# 17. Key Interview Points

Important points to remember:

* API Server is the central component
* etcd stores cluster state
* Scheduler assigns pods to nodes
* Controller Manager maintains desired state
* kubelet runs containers on nodes
* kube-proxy manages networking

---

# 18. Summary

Kubernetes architecture separates cluster management from application execution.

Control Plane → Manages cluster

Worker Nodes → Run applications

Understanding architecture is critical before learning pods, deployments, and services.
