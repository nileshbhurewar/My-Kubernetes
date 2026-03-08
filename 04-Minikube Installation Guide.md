# Minikube Installation Guide (Ubuntu)

This guide explains how to install **Minikube** and run a local Kubernetes cluster on an Ubuntu EC2 instance (or VM).

---

## Prerequisites

* Ubuntu 20.04+ (Xenial or later also works)
* `t2.medium` (2 vCPU, 4 GB RAM) or larger instance type
* User: `ubuntu` with `sudo` privileges
* Internet access

---

## 🛠 Installation Steps

### 1. Update & Install Dependencies

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install -y apt-transport-https ca-certificates curl docker.io conntrack
```

### 2. Enable Docker

```bash
sudo systemctl enable --now docker
```

Add `ubuntu` user to Docker group:

```bash
sudo usermod -aG docker ubuntu
```

Apply group changes:

```bash
newgrp docker
```

---

### 3. Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -m 0755 kubectl /usr/local/bin/kubectl
rm -f kubectl
```

---

### 4. Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
rm -f minikube-linux-amd64
```

---

### 5. Start Minikube

#### Auto-detect CPU & RAM

```bash
CPUS=$(nproc)
TOTAL_RAM_MB=$(free -m | awk '/^Mem:/{print $2}')
ALLOC_RAM_MB=$(( TOTAL_RAM_MB * 75 / 100 ))
```

#### Start Minikube as `ubuntu` user

```bash
minikube start --driver=docker --cpus=${CPUS} --memory=${ALLOC_RAM_MB} --force
```

---

### 6. Enable Useful Addons

```bash
minikube addons enable ingress
minikube addons enable metrics-server
```

---

## 🧪 Verify Installation

Check Minikube status:

```bash
minikube status
```

Check Kubernetes nodes:

```bash
kubectl get nodes
```

Expected output:

```bash
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   1m    v1.29.x
```

---

## Common Issues

### 1. Permission denied with Docker

```bash
sudo usermod -aG docker ubuntu
newgrp docker
```

### 2. Minikube fails to start

Make sure Docker is running:

```bash
systemctl status docker
```

### 3. Low resources

Use at least `t2.medium` (2 vCPUs, 4 GB RAM). Increase resources manually:

```bash
minikube start --driver=docker --cpus=2 --memory=4096
```

---

## Next Steps

### Deploy a sample app

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
minikube service nginx
```
