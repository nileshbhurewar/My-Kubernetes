# 11 - Volumes

## 1. Introduction

In Kubernetes, containers inside pods are **ephemeral**. This means that when a container crashes or a pod is deleted, all data stored inside the container filesystem is lost.

To solve this problem, Kubernetes provides **Volumes**.

A **Volume** is a storage mechanism that allows data to persist beyond the lifecycle of individual containers.

Volumes allow:

* Persistent data storage
* Data sharing between containers
* External storage integration

---

# 2. Why Volumes Are Needed

Without volumes, applications face major issues:

1. Data loss when containers restart
2. No way to share files between containers
3. No persistent storage for databases

Example:

A MySQL container storing database files inside the container filesystem.

If the pod crashes → database files are lost.

Using a **volume**, the database files are stored outside the container so they survive restarts.

---

# 3. How Kubernetes Volumes Work

Volumes are defined in the **Pod specification**.

They are mounted inside containers as directories.

Structure:

Pod
├── Container
│     └── Mounted Volume (/data)
└── Volume (storage location)

Containers read and write data through the mounted directory.

---

# 4. Volume YAML Structure

Example Pod with Volume:

apiVersion: v1
kind: Pod
metadata:
name: volume-example
spec:
containers:

* name: nginx
  image: nginx
  volumeMounts:

  * name: nginx-storage
    mountPath: /usr/share/nginx/html
    volumes:
* name: nginx-storage
  emptyDir: {}

Important fields:

volumes → defines storage

volumeMounts → mounts storage inside container

mountPath → directory where storage appears in container

---

# 5. emptyDir Volume

emptyDir is a temporary volume created when a pod starts.

Characteristics:

* Created when pod starts
* Deleted when pod is removed
* Shared between containers in same pod

Example use cases:

* Cache storage
* Temporary files
* Data sharing between containers

Example:

volumes:

* name: cache-volume
  emptyDir: {}

---

# 6. hostPath Volume

hostPath mounts a file or directory from the **host node filesystem** into the pod.

Example:

volumes:

* name: host-storage
  hostPath:
  path: /data
  type: Directory

Use cases:

* Access host system logs
* Access node-level data

Important note:

hostPath is not recommended for production clusters because it ties pods to specific nodes.

---

# 7. Persistent Volumes (PV)

A **Persistent Volume (PV)** is a piece of storage in the cluster provisioned by an administrator.

It represents real storage resources such as:

* NFS
* AWS EBS
* Azure Disk
* Google Persistent Disk

Example PV YAML:

apiVersion: v1
kind: PersistentVolume
metadata:
name: pv-volume
spec:
capacity:
storage: 1Gi
accessModes:
- ReadWriteOnce
hostPath:
path: /mnt/data

---

# 8. Persistent Volume Claims (PVC)

A **Persistent Volume Claim (PVC)** is a request for storage by a user.

Instead of using storage directly, pods request storage through PVCs.

PVC automatically binds to a suitable Persistent Volume.

Example PVC YAML:

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
name: pvc-example
spec:
accessModes:
- ReadWriteOnce
resources:
requests:
storage: 500Mi

---

# 9. Using PVC in a Pod

Example Pod using PVC:

apiVersion: v1
kind: Pod
metadata:
name: pvc-pod
spec:
containers:

* name: app
  image: nginx
  volumeMounts:

  * mountPath: "/data"
    name: storage
    volumes:
* name: storage
  persistentVolumeClaim:
  claimName: pvc-example

---

# 10. Access Modes

Persistent volumes support different access modes.

### ReadWriteOnce (RWO)

Volume can be mounted as read-write by **one node**.

### ReadOnlyMany (ROX)

Volume can be mounted as read-only by **multiple nodes**.

### ReadWriteMany (RWX)

Volume can be mounted as read-write by **multiple nodes**.

---

# 11. Storage Classes

StorageClass allows **dynamic storage provisioning**.

Instead of manually creating PVs, Kubernetes automatically creates storage.

Example:

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
name: fast-storage
provisioner: kubernetes.io/aws-ebs

When PVC is created, Kubernetes dynamically provisions a volume.

---

# 12. Volume Lifecycle

Volume lifecycle depends on its type.

For example:

emptyDir → deleted with pod

PersistentVolume → survives pod deletion

This allows applications to maintain persistent data.

---

# 13. Real World Example

Example: Running a MySQL database in Kubernetes.

Database requires persistent storage.

Steps:

1. Create Persistent Volume
2. Create Persistent Volume Claim
3. Mount PVC into MySQL container

Now database files remain safe even if the pod restarts.

---

# 14. Common Volume Types

Important volume types:

* emptyDir
* hostPath
* PersistentVolume
* NFS
* AWS EBS
* Azure Disk
* Google Persistent Disk

Each type provides different storage capabilities.

---

# 15. Best Practices

Recommended practices:

* Use Persistent Volumes for databases
* Avoid hostPath in production
* Use StorageClasses for dynamic provisioning
* Separate compute and storage

---

# 16. Important Interview Questions

### What is a volume in Kubernetes?

A volume is a storage mechanism that allows data to persist and be shared between containers in a pod.

### What is the difference between PV and PVC?

Persistent Volume is the actual storage resource.

Persistent Volume Claim is a request for that storage.

### Why are volumes important in Kubernetes?

They provide persistent storage so application data is not lost when pods restart.

---

# 17. Summary

Volumes allow Kubernetes applications to store persistent data.

Key concepts:

* emptyDir for temporary storage
* hostPath for node storage
* Persistent Volumes for long-term storage
* Persistent Volume Claims for requesting storage

Understanding Kubernetes storage is critical for running stateful applications like databases.
