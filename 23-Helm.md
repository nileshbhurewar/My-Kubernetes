# 22 - Helm

## 1. Introduction

Helm is a **package manager for Kubernetes** that helps you define, install, and manage Kubernetes applications.

Just like:

* apt for Ubuntu
* yum for RedHat
* npm for Node.js

Helm manages Kubernetes applications using **Helm Charts**.

Helm simplifies the deployment of complex Kubernetes applications that require multiple YAML files.

---

# 2. Why Helm is Needed

In real Kubernetes projects, applications consist of many resources such as:

* Deployments
* Services
* ConfigMaps
* Secrets
* Ingress
* Persistent Volumes

Managing many YAML files manually becomes difficult.

Helm solves this by:

* Packaging Kubernetes manifests
* Allowing version control
* Enabling easy upgrades and rollbacks

---

# 3. Helm Architecture

Helm works using three main concepts:

Helm Client

Charts

Releases

### Helm Client

Helm CLI tool used to interact with Kubernetes clusters.

### Charts

A Helm Chart is a **collection of Kubernetes YAML templates** used to deploy an application.

### Release

A release is a running instance of a chart inside a Kubernetes cluster.

---

# 4. Helm Installation

Install Helm on Linux:

curl [https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3](https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3) | bash

Verify installation:

helm version

---

# 5. Helm Repository

Helm charts are stored in repositories.

Example public repository:

Helm Stable Repo

Add repository:

helm repo add bitnami [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)

Update repository:

helm repo update

Search charts:

helm search repo nginx

---

# 6. Installing Applications with Helm

Example installing nginx:

helm install my-nginx bitnami/nginx

Check releases:

helm list

This installs all required Kubernetes resources automatically.

---

# 7. Helm Chart Structure

A Helm chart has a predefined directory structure.

Example structure:

Chart.yaml

values.yaml

templates/

charts/

### Chart.yaml

Contains metadata about the chart.

Example:

apiVersion: v2
name: myapp
version: 1.0.0

### values.yaml

Contains configurable values for the chart.

Example:

replicaCount: 2

image:
repository: nginx
tag: latest

### templates/

Contains Kubernetes YAML templates.

---

# 8. Creating a Helm Chart

Create new chart:

helm create mychart

Directory structure generated automatically.

Deploy chart:

helm install myapp ./mychart

---

# 9. Helm Values

Values allow configuration of charts without modifying templates.

Example:

helm install myapp ./mychart --values values.yaml

Override values directly:

helm install myapp ./mychart --set replicaCount=3

---

# 10. Helm Upgrade

Upgrade existing release:

helm upgrade myapp ./mychart

This updates the application with new configurations.

---

# 11. Helm Rollback

Helm supports rollback to previous versions.

Check history:

helm history myapp

Rollback release:

helm rollback myapp 1

This restores the previous deployment version.

---

# 12. Uninstall Helm Release

Remove application:

helm uninstall myapp

This deletes all Kubernetes resources created by the chart.

---

# 13. Helm Template Engine

Helm uses the **Go templating engine**.

Example template:

{{ .Values.replicaCount }}

This allows dynamic configuration of Kubernetes manifests.

Example deployment template:

replicas: {{ .Values.replicaCount }}

---

# 14. Helm Benefits

Key advantages:

* Simplifies Kubernetes deployments
* Supports version control
* Easy upgrades and rollbacks
* Reusable application templates

Helm is widely used in production Kubernetes environments.

---

# 15. Real World Example

Example deploying Prometheus using Helm.

Instead of writing many YAML files manually:

helm install monitoring prometheus-community/prometheus

Helm automatically deploys:

* Prometheus server
* Alertmanager
* Service
* ConfigMaps

---

# 16. Best Practices

Recommended practices:

* Store Helm charts in Git repositories
* Use versioning for charts
* Avoid hardcoding values
* Use values.yaml for configuration

---

# 17. Important Interview Questions

### What is Helm in Kubernetes?

Helm is a package manager for Kubernetes used to define, install, and manage Kubernetes applications using Helm charts.

### What is a Helm Chart?

A Helm chart is a package containing Kubernetes YAML templates required to deploy an application.

### What is a Helm Release?

A release is an instance of a Helm chart deployed in a Kubernetes cluster.

### What is values.yaml?

values.yaml stores configuration values that customize Helm charts.

---

# 18. Summary

Helm simplifies application deployment in Kubernetes by packaging resources into reusable charts.

Key concepts:

* Helm CLI
* Charts
* Releases
* Templates
* values.yaml

Helm is an essential tool for managing complex Kubernetes applications efficiently.
