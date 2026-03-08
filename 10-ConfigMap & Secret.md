# 🔐 Kubernetes ConfigMap & Secret Notes

## 📘 Overview

**ConfigMaps** and **Secrets** are Kubernetes resources used to manage configuration data and sensitive information **separately from application code**.

* **ConfigMap:** Stores **non-confidential** key-value pairs (e.g., environment variables, config files).
* **Secret:** Stores **sensitive** data like passwords, tokens, and certificates — automatically **Base64 encoded**.

These are essential for making applications **configurable, secure, and portable**.

---

## 🧩 1. ConfigMap

### 📄 Purpose

* To store **configuration data** that can be injected into pods as **environment variables** or **configuration files**.
* Helps in **decoupling config from images**, making the application more flexible.

### 🔧 Create ConfigMap

There are **three ways** to create a ConfigMap:

#### 1️⃣ From Literal Values

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false
```

#### 2️⃣ From a File

```bash
kubectl create configmap app-config --from-file=app.properties
```

#### 3️⃣ From a YAML Manifest

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: dev
  labels:
    app: demo
  annotations:
    purpose: store-non-sensitive-config

data:
  APP_ENV: production
  APP_DEBUG: "false"
  APP_VERSION: "1.0"
```

---

### 🧠 Using ConfigMap in Pods

#### Inject as Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
spec:
  containers:
    - name: demo
      image: nginx
      envFrom:
        - configMapRef:
            name: app-config
```

#### Inject as Volume Mount

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
spec:
  containers:
    - name: demo
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

#### Verify Inside Pod

```bash
kubectl exec -it demo-pod -- env | grep APP
```

---

## 🧩 2. Secrets

### 🔐 Purpose

* To store **confidential data** like passwords, API keys, tokens, or certificates.
* Secrets are **Base64 encoded**, not encrypted by default (but can be encrypted at rest using KMS).

### 🔧 Create Secret

There are multiple ways to create a Secret:

#### 1️⃣ From Literal Values

```bash
kubectl create secret generic db-secret \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASS=admin123
```

#### 2️⃣ From a File

```bash
kubectl create secret generic ssl-cert --from-file=server.crt --from-file=server.key
```

#### 3️⃣ From a YAML Manifest

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: dev
type: Opaque
data:
  DB_USER: YWRtaW4=        # echo -n 'admin' | base64
  DB_PASS: YWRtaW4xMjM=    # echo -n 'admin123' | base64
```

---

### 🧠 Using Secret in Pods

#### Inject as Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
spec:
  containers:
    - name: db-container
      image: mysql:8.0
      env:
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_USER
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_PASS
```

#### Inject as Volume Mount

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-volume-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: secret-vol
          mountPath: /etc/secrets
          readOnly: true
  volumes:
    - name: secret-vol
      secret:
        secretName: db-secret
```

---

## 🧾 Verify ConfigMap & Secret

```bash
kubectl get configmaps -n dev
kubectl get secrets -n dev
kubectl describe configmap app-config
kubectl describe secret db-secret
```

To decode secret data:

```bash
echo 'YWRtaW4xMjM=' | base64 --decode
```

---

## ⚙️ Best Practices

✅ Keep secrets small and only store sensitive data.
✅ Use **RBAC** to restrict access to secrets.
✅ Enable **encryption at rest** for secrets in production clusters.
✅ Mount secrets as volumes instead of printing in logs.
✅ Use external secret managers (e.g., AWS Secrets Manager, HashiCorp Vault) for large-scale setups.

---

## 🧩 Related Topics

* ConfigMap vs Secret comparison
* Kubernetes Volumes and Persistent Storage
* RBAC for Secrets management
* Secret encryption using KMS
* External Secret Management (Vault, AWS Secrets Manager)
