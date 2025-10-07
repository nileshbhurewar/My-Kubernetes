# 🧠 Kubernetes StatefulSet Notes

## 📘 Overview

**StatefulSets** are used to manage stateful applications in Kubernetes, such as **databases (MySQL, MongoDB, PostgreSQL)**. Unlike Deployments, each pod in a StatefulSet has a **unique, stable identity** and maintains its **state even after restarts**.

---

## ⚙️ Key Concepts

* **Ordered & Numbered Pods** – Pods are created sequentially (`pod-0`, `pod-1`, `pod-2`...).
* **Persistent State** – When a pod dies, its data is preserved and attached to the new pod with the same identity.
* **Volume Usage** – StatefulSets require persistent volumes to store data.
* **Stable Network ID** – Each pod gets a predictable DNS name.
* **Commonly used for:**

  * Databases: MySQL, MongoDB, PostgreSQL
  * Message Queues: Kafka, RabbitMQ

---

## 🧩 Example: MySQL StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-statefulset
spec:
  serviceName: mysql-service
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
            - name: MYSQL_DATABASE
              value: devops
          volumeMounts:
            - name: mysql-data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

---

## 🧱 Headless Service

A **Headless Service** is used with StatefulSets to provide stable network identities for pods.

* **`clusterIP: None`** → No cluster-wide IP; instead, each pod gets its own DNS record.
* Used for **internal communication** between pods.

### Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
  namespace: mysql
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
    - name: mysql
      protocol: TCP
      port: 3306
      targetPort: 3306
```

---

## 🚀 Deployment Steps

1. Apply the StatefulSet and Service files:

   ```bash
   kubectl apply -f statefulset.yml
   kubectl apply -f service.yml
   ```
2. Verify Pods and Services:

   ```bash
   kubectl get pods -n mysql
   kubectl get svc -n mysql
   ```
3. Access the MySQL pod:

   ```bash
   kubectl exec -it pod/mysql-statefulset-0 -n mysql -- bash
   mysql -u root -p
   # password: root
   ```
4. Verify database:

   ```sql
   SHOW DATABASES;
   ```
5. Delete any pod (e.g., `mysql-statefulset-0`) and see it recreated automatically with the same identity.

---

## 📦 Related Topics

* ConfigMaps & Secrets
* Persistent Volumes (PV) and Persistent Volume Claims (PVC)
* Stateful Application Scaling
* Headless vs ClusterIP Services
