# 21 - Kubernetes Networking

## 1. Introduction

Kubernetes networking defines how communication happens between different components inside a Kubernetes cluster.

Networking allows:

* Pod to Pod communication
* Pod to Service communication
* External traffic to reach applications

Kubernetes networking follows a simple principle:

"Every pod should be able to communicate with every other pod without NAT."

---

# 2. Kubernetes Networking Model

The Kubernetes networking model is based on three fundamental requirements:

1. Pods can communicate with other pods without NAT
2. Nodes can communicate with all pods
3. Pods see their own IP address

This model ensures seamless communication across the cluster.

---

# 3. Pod Networking

Each pod in Kubernetes gets its own **unique IP address**.

Important characteristics:

* Pods share network namespace within the same pod
* Containers inside a pod communicate using **localhost**

Example:

Container A → localhost → Container B

Pods communicate with other pods using their **pod IP addresses**.

---

# 4. Pod-to-Pod Communication

Pods can communicate across nodes without manual configuration.

Example scenario:

Node 1
Pod A → 10.244.1.2

Node 2
Pod B → 10.244.2.5

Pod A can directly communicate with Pod B using the pod IP.

This functionality is provided by **Container Network Interface (CNI) plugins**.

---

# 5. Container Network Interface (CNI)

Kubernetes itself does not implement networking.

Instead it uses **CNI plugins**.

CNI plugins configure networking for pods.

Popular CNI plugins include:

* Calico
* Flannel
* Weave
* Cilium

Example responsibilities of CNI:

* Assign pod IP addresses
* Configure routing
* Enable pod communication

---

# 6. Cluster Networking Components

Important networking components include:

Pods

Services

kube-proxy

CNI plugin

Ingress

These components work together to manage communication inside and outside the cluster.

---

# 7. Services

Pods are ephemeral and their IP addresses can change.

To provide stable communication, Kubernetes uses **Services**.

A Service provides:

* Stable IP address
* DNS name
* Load balancing

Example:

Multiple pods running application.

Service routes traffic to available pods.

Service types:

ClusterIP

NodePort

LoadBalancer

ExternalName

---

# 8. kube-proxy

kube-proxy runs on every node.

Responsibilities:

* Maintains network rules
* Enables service networking
* Load balances traffic to pods

kube-proxy uses:

* iptables
* IPVS

When a request reaches a service, kube-proxy forwards it to the correct pod.

---

# 9. DNS in Kubernetes

Kubernetes provides built-in DNS for service discovery.

CoreDNS is typically used as the cluster DNS service.

Example:

If a service is named:

my-service

It can be accessed using:

my-service.default.svc.cluster.local

Pods can resolve service names automatically.

---

# 10. Ingress

Ingress manages **external access to services**.

It allows HTTP and HTTPS routing to services inside the cluster.

Example:

Internet
↓
Ingress Controller
↓
Service
↓
Pods

Ingress supports:

* Host-based routing
* Path-based routing
* TLS termination

Example Ingress rule:

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
name: example-ingress
spec:
rules:

* host: myapp.com
  http:
  paths:

  * path: /
    pathType: Prefix
    backend:
    service:
    name: my-service
    port:
    number: 80

---

# 11. Network Policies

Network policies control traffic between pods.

They define **which pods can communicate with other pods**.

Example policy:

Allow only pods with label app=frontend to access backend pods.

Example YAML:

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: allow-frontend
spec:
podSelector:
matchLabels:
app: backend
ingress:

* from:

  * podSelector:
    matchLabels:
    app: frontend

This restricts access to backend pods.

---

# 12. External Traffic Flow

Typical flow of external traffic:

User Request
↓
Load Balancer / NodePort
↓
Service
↓
Pods

If using Ingress:

User
↓
Ingress Controller
↓
Service
↓
Pods

---

# 13. Real World Example

Example: Deploying a web application.

1. Application runs in pods
2. Service exposes pods internally
3. Ingress exposes service externally

Traffic flow:

Internet → Ingress → Service → Pods

This architecture ensures scalability and load balancing.

---

# 14. Networking Debugging Commands

Check pods with IPs:

kubectl get pods -o wide

Check services:

kubectl get svc

Check endpoints:

kubectl get endpoints

Check network policies:

kubectl get networkpolicy

Check DNS resolution:

kubectl exec -it <pod-name> -- nslookup my-service

---

# 15. Best Practices

Recommended practices:

* Use network policies for security
* Use services instead of direct pod communication
* Monitor CNI plugin health
* Use ingress for HTTP routing

Proper networking design is essential for secure and scalable Kubernetes applications.

---

# 16. Important Interview Questions

### What is Kubernetes networking?

Kubernetes networking defines how pods communicate with each other, with services, and with external clients.

### What is CNI?

CNI (Container Network Interface) is a plugin that provides networking capabilities for Kubernetes pods.

### Why are services needed?

Services provide a stable IP address and DNS name to access pods whose IP addresses may change.

### What is an Ingress?

Ingress is a Kubernetes resource that manages external HTTP/HTTPS access to services inside the cluster.

---

# 17. Summary

Kubernetes networking enables communication between pods, services, and external clients.

Key concepts:

* Pod networking
* CNI plugins
* Services
* kube-proxy
* DNS
* Ingress
* Network Policies

Understanding networking is essential for building scalable and secure Kubernetes applications.
