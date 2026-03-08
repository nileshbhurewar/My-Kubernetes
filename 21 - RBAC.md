# 21 - RBAC (Role Based Access Control)

## 1. Introduction

RBAC (Role Based Access Control) is a Kubernetes security mechanism used to control **who can access the Kubernetes cluster and what actions they can perform**.

RBAC allows administrators to define permissions based on roles instead of assigning permissions directly to users.

RBAC controls access to:

* Pods
* Deployments
* Services
* ConfigMaps
* Secrets
* Nodes
* Namespaces

RBAC is essential for **cluster security in production environments**.

---

# 2. Why RBAC is Needed

In real Kubernetes clusters multiple users interact with the system.

Examples:

* Developers
* DevOps engineers
* QA engineers
* Cluster administrators

Without RBAC:

Any user could modify or delete critical resources.

RBAC ensures:

* Controlled access
* Least privilege principle
* Secure cluster management

---

# 3. RBAC Core Components

RBAC is built using four main resources:

1. Role
2. ClusterRole
3. RoleBinding
4. ClusterRoleBinding

---

# 4. Role

A **Role** defines permissions within a specific namespace.

It specifies what actions are allowed on which resources.

Example Role:

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
namespace: dev
name: pod-reader
rules:

* apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

This role allows users to **view pods inside the dev namespace**.

---

# 5. ClusterRole

A **ClusterRole** defines permissions across the entire cluster.

It is not limited to a namespace.

Example:

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
name: node-reader
rules:

* apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "list", "watch"]

ClusterRoles are used for resources like:

* nodes
* persistent volumes
* cluster level monitoring

---

# 6. RoleBinding

A **RoleBinding** connects a Role with a user, group, or service account.

It grants permissions defined in the role.

Example:

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
name: read-pods
namespace: dev
subjects:

* kind: User
  name: developer
  apiGroup: rbac.authorization.k8s.io
  roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io

This allows the user **developer** to read pods in the dev namespace.

---

# 7. ClusterRoleBinding

A **ClusterRoleBinding** grants cluster-wide permissions.

It connects a ClusterRole with users or groups.

Example:

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
name: cluster-admin-binding
subjects:

* kind: User
  name: admin
  apiGroup: rbac.authorization.k8s.io
  roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io

This grants **full cluster admin privileges**.

---

# 8. RBAC Workflow

RBAC permission flow works like this:

User
↓
Authentication
↓
Authorization (RBAC)
↓
Allowed / Denied

Steps:

1. User sends request
2. API server authenticates user
3. RBAC checks permissions
4. Request allowed or rejected

---

# 9. Verbs in RBAC

Verbs define allowed actions.

Common verbs:

get → read a resource

list → list resources

watch → monitor changes

create → create resource

update → modify resource

delete → remove resource

patch → partial update

Example:

verbs: ["get", "list", "watch"]

---

# 10. Resources in RBAC

Resources define which Kubernetes objects can be accessed.

Examples:

pods
services
deployments
configmaps
secrets
nodes

Example rule:

resources: ["pods"]

---

# 11. Checking RBAC Permissions

You can test permissions using:

kubectl auth can-i

Example:

kubectl auth can-i create pods

Check specific user permission:

kubectl auth can-i delete pods --as developer

---

# 12. Viewing RBAC Resources

List roles:

kubectl get roles

List rolebindings:

kubectl get rolebindings

List clusterroles:

kubectl get clusterroles

List clusterrolebindings:

kubectl get clusterrolebindings

---

# 13. Real World Example

Example scenario:

A developer should only:

* view pods
* view logs

But should NOT:

* delete deployments
* modify services

Solution:

Create a Role with limited permissions and bind it to the developer using RoleBinding.

---

# 14. RBAC Best Practices

Recommended practices:

* Follow **least privilege principle**
* Avoid giving cluster-admin access
* Use namespace-specific roles
* Use service accounts for applications

RBAC should be carefully designed for secure clusters.

---

# 15. RBAC vs ABAC

RBAC

* Role based
* More structured
* Widely used in Kubernetes

ABAC (Attribute Based Access Control)

* Policy based
* Less commonly used

Kubernetes recommends RBAC.

---

# 16. Important Interview Questions

### What is RBAC in Kubernetes?

RBAC is a security mechanism used to control access to Kubernetes resources based on roles.

### What are the components of RBAC?

Role, ClusterRole, RoleBinding, ClusterRoleBinding.

### Difference between Role and ClusterRole?

Role works within a namespace.

ClusterRole works across the entire cluster.

### Difference between RoleBinding and ClusterRoleBinding?

RoleBinding grants permissions within a namespace.

ClusterRoleBinding grants permissions across the cluster.

---

# 17. Summary

RBAC is a critical security feature in Kubernetes.

Key concepts:

* Role defines permissions
* RoleBinding assigns permissions
* ClusterRole provides cluster-wide permissions
* ClusterRoleBinding grants cluster access

RBAC ensures secure and controlled access to Kubernetes resources.
