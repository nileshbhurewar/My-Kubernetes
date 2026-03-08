# 01 - Kubernetes Basics

## 1. What is Kubernetes?

Kubernetes (often abbreviated as **K8s**) is an open-source container orchestration platform used to automate the deployment, scaling, and management of containerized applications.

It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

Kubernetes helps manage applications that run inside containers such as Docker containers.

Instead of manually managing containers across multiple servers, Kubernetes automatically:

* Deploys containers
* Scales applications
* Handles failures
* Balances traffic
* Manages networking

## 2. Why Kubernetes is Needed

When applications grow large, running containers manually becomes difficult.

Problems without Kubernetes:

1. Containers crash and require manual restart
2. Traffic load increases but containers cannot scale automatically
3. Managing hundreds of containers across multiple servers becomes complex
4. Networking between containers becomes difficult
5. Updating applications without downtime is hard

Kubernetes solves these problems using automation.

## 3. Containerization Recap

Before understanding Kubernetes, it is important to understand containers.

A **container** is a lightweight package that includes:

* Application code
* Runtime
* Libraries
* Dependencies

Example:

A Java application container may contain:

* JDK
* Application JAR file
* Required libraries

Docker is commonly used to create and run containers.

Example Docker command:

```
docker run nginx
```

But managing thousands of containers using Docker alone is difficult.

This is where Kubernetes helps.

## 4. Kubernetes vs Docker

Docker and Kubernetes are often confused but they solve different problems.

Docker:

* Used to build container images
* Used to run containers

Kubernetes:

* Used to manage containers at scale
* Orchestrates containers across multiple servers

## 5. Key Features of Kubernetes

### 1. Container Orchestration

Automatically manages container lifecycle.

### 2. Self Healing

If a container crashes, Kubernetes automatically restarts it.

If a node fails, containers are scheduled on another node.

### 3. Auto Scaling

Applications can scale based on CPU or memory usage.

### 4. Load Balancing

Traffic is automatically distributed across multiple containers.

### 5. Rolling Updates

Applications can be updated without downtime.

### 6. Service Discovery

Containers can communicate using internal DNS.

## 6. Kubernetes Architecture Overview

A Kubernetes cluster consists of:

* Control Plane (Master Node)
* Worker Nodes

Control Plane manages the cluster.

Worker Nodes run applications.

## 7. Kubernetes Cluster

A **Kubernetes Cluster** is a group of machines working together to run containerized applications.

Cluster contains:

* Control Plane
* Worker Nodes

## 8. Node

A **Node** is a machine (physical or virtual) that runs containers.

Types of nodes:

1. Master Node (Control Plane)
2. Worker Node

Worker nodes run the application containers.

## 9. Pod

A **Pod** is the smallest deployable unit in Kubernetes.

A pod can contain:

* One container
* Multiple containers

Containers in the same pod share:

* Network
* Storage

Example Pod:

```
Pod
 └── Container (nginx)
```

## 10. Kubernetes Objects

Kubernetes works using objects.

Important objects include:

* Pod
* ReplicaSet
* Deployment
* Service
* ConfigMap
* Secret
* Volume

These objects are usually defined using **YAML files**.

Example YAML structure:

```
apiVersion:
kind:
metadata:
spec:
```

## 11. Kubernetes YAML File Structure

Example Pod YAML:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
```

Apply configuration using:

```
kubectl apply -f pod.yaml
```

## 12. Declarative vs Imperative Approach

### Imperative

Direct commands.

Example:

```
kubectl run nginx --image=nginx
```

### Declarative

Using YAML files.

Example:

```
kubectl apply -f deployment.yaml
```

Declarative approach is recommended for production.

## 13. Kubernetes Workflow

Typical Kubernetes workflow:

1. Build Docker image
2. Push image to container registry
3. Create Kubernetes YAML file
4. Deploy using kubectl
5. Kubernetes schedules pods on nodes

## 14. Kubernetes Components (High Level)

Control Plane Components:

* API Server
* etcd
* Controller Manager
* Scheduler

Node Components:

* kubelet
* kube-proxy
* container runtime

Detailed architecture will be covered in the next section.

## 15. Kubernetes Use Cases

Kubernetes is widely used for:

* Microservices architecture
* CI/CD pipelines
* Cloud native applications
* Large scale container deployments

## 16. Kubernetes Advantages

* High availability
* Automated scaling
* Self healing
* Infrastructure abstraction
* Easy cloud deployment

## 17. Kubernetes Disadvantages

* Complex to learn
* Requires proper cluster management
* Networking concepts can be difficult

## 18. Kubernetes Learning Path

Typical Kubernetes learning sequence:

1. Kubernetes Basics
2. Architecture
3. Cluster Setup
4. kubectl
5. Pods
6. ReplicaSets
7. Deployments
8. Services
9. Ingress
10. ConfigMaps & Secrets
11. Volumes
12. StatefulSets
13. DaemonSets
14. Jobs & CronJobs
15. RBAC
16. Autoscaling
17. Networking
18. Helm
19. Monitoring
20. Troubleshooting

## 19. Kubernetes Real World Example

Example application deployment:

User Request → Load Balancer → Service → Pods → Containers

Multiple pods ensure application availability.

## 20. Summary

Kubernetes is a powerful container orchestration system that automates application deployment, scaling, and management.

Key Concepts:

* Cluster
* Node
* Pod
* Container
* Kubernetes Objects

These fundamentals are required before moving to deeper Kubernetes topics.
