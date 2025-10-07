🚀 Top Kubernetes Interview Questions & Answers (2025 Edition)
🧩 1. What is Kubernetes?

Answer:
Kubernetes (K8s) is an open-source container orchestration platform that automates deployment, scaling, and management of containerized applications.

Key Point: It ensures high availability, scalability, and self-healing of applications.

⚙️ 2. What is a Pod in Kubernetes?

Answer:
A Pod is the smallest deployable unit in Kubernetes that contains one or more containers that share the same network namespace, IP address, and storage.

🧠 Example: One NGINX container + one sidecar logging container in a single Pod.

📦 3. What is the difference between a Pod and a Deployment?

Answer:

Pod	Deployment
Basic unit that runs containers	Manages multiple replicas of Pods
Can crash and not auto-recover	Ensures desired number of Pods are always running
Used for testing/simple apps	Used for production workloads
🔁 4. What is a ReplicaSet?

Answer:
A ReplicaSet ensures a specified number of identical Pods are running at all times.
If a Pod fails or is deleted, ReplicaSet automatically creates a new one.

🏗️ 5. What is a Deployment in Kubernetes?

Answer:
A Deployment manages ReplicaSets and provides features like rolling updates, rollbacks, scaling, and version management of applications.

🌐 6. What are the different types of Services in Kubernetes?

Answer:

Type	Description
ClusterIP	Default type — exposes the service internally within the cluster
NodePort	Exposes the service on a static port (30000–32767) of each node
LoadBalancer	Uses an external cloud load balancer to expose service to the internet
🗂️ 7. What is a Namespace?

Answer:
Namespace is used to logically isolate resources in a cluster.
Example: dev, test, and prod environments in the same cluster.

⚙️ 8. What are Labels and Selectors?

Answer:

Labels: Key-value pairs attached to objects (like Pods).

Selectors: Used by Services, ReplicaSets, etc. to identify Pods based on labels.

🧠 Example:

labels:
  app: nginx

🔐 9. What is the difference between ConfigMap and Secret?
ConfigMap	Secret
Stores non-confidential data like environment variables	Stores sensitive data like passwords or tokens
Stored in plain text	Base64-encoded by default
💾 10. What is a Volume in Kubernetes?

Answer:
A Volume is used to persist data beyond the container lifecycle.
When a Pod dies, normal container storage is lost — but Volume keeps the data.

☁️ 11. What is the difference between PersistentVolume (PV) and PersistentVolumeClaim (PVC)?
PV	PVC
The actual physical storage (NFS, AWS EBS, etc.)	A user’s request for storage
Created by admin	Requested by developer
🔄 12. What is a DaemonSet?

Answer:
DaemonSet ensures that a copy of a Pod runs on every node in the cluster.
Example: Running logging agents like Fluentd or monitoring agents like Datadog.

💡 13. What is a StatefulSet?

Answer:
Used for stateful applications that need stable, unique network identities and persistent storage (like databases: MySQL, MongoDB).

🧰 14. What is a Job and a CronJob?

Job: Runs a task once until completion (e.g., batch processing).

CronJob: Runs Jobs periodically based on a schedule (like a Linux cron).

🧭 15. What is an Ingress and Ingress Controller?

Answer:

Ingress: Manages external HTTP/HTTPS access to Services inside the cluster using rules.

Ingress Controller: Software (like NGINX) that implements the Ingress rules.

🛠️ 16. How do you access a Pod’s application from outside the cluster?

Answer:

NodePort service

LoadBalancer service

Port Forwarding:

kubectl port-forward service/myapp 8080:80 --address=0.0.0.0

🧩 17. What is a kubelet?

Answer:
Kubelet is an agent running on each node that ensures containers described in PodSpecs are running and healthy.

📋 18. What is the role of etcd?

Answer:
etcd is a key-value store that holds the entire cluster state — configuration, secrets, and metadata.

⚖️ 19. How do you scale a Deployment?

Answer:

kubectl scale deployment myapp --replicas=3


or edit the YAML:

replicas: 3

🔁 20. What is a Rolling Update and Rollback?

Answer:

Rolling Update: Gradually replaces old Pods with new ones — ensures zero downtime.

Rollback: Revert to a previous working version if the update fails.

kubectl rollout undo deployment/myapp

🧠 21. What happens when you run kubectl apply -f deployment.yaml?

Answer:

The YAML is sent to the API server.

The API server updates etcd.

The Scheduler assigns Pods to nodes.

Kubelet pulls images and starts containers.

🩺 22. How do you troubleshoot a Pod in CrashLoopBackOff?

Answer:

kubectl describe pod <pod-name>
kubectl logs <pod-name>


Check for:

Wrong command in YAML

App not listening on correct port

Missing dependencies or configs

📡 23. How do Pods communicate with each other?

Answer:
Through Cluster networking (CNI plugin) — each Pod has its own IP, and all Pods can communicate within the same cluster without NAT.

🧮 24. What is a Node in Kubernetes?

Answer:
A Node is a worker machine (physical or virtual) that runs Pods. It contains kubelet, kube-proxy, and a container runtime (like Docker or containerd).

🧰 25. What is Helm in Kubernetes?

Answer:
Helm is a package manager for Kubernetes used to define, install, and manage complex applications through reusable charts.

⚡ Bonus: Real-World DevOps Questions
Question	Short Answer
How do you deploy an app using CI/CD to K8s?	Build Docker image → push to registry → apply manifests via Jenkins or GitHub Actions
How do you monitor a K8s cluster?	Using tools like Datadog, Prometheus, Grafana, or CloudWatch
How do you secure a cluster?	RBAC, Secrets, NetworkPolicies, private registries, restricted ServiceAccounts
⚙️ 26. What are the main components of the Kubernetes architecture?

Answer:

Component	Role
API Server	Frontend for Kubernetes — handles REST requests
etcd	Key-value store for cluster data
Scheduler	Assigns Pods to suitable Nodes
Controller Manager	Maintains cluster state (replicas, nodes, etc.)
Kubelet	Ensures Pods are running on the node
Kube-proxy	Handles networking/routing for Pods
🌐 27. What is the role of the Kubernetes API Server?

Answer:
It’s the entry point for all K8s operations — validates requests, updates etcd, and communicates with other components.
Every kubectl command interacts with the API server.

🧩 28. How does the Kubernetes Scheduler work?

Answer:
It watches for unscheduled Pods and assigns them to the most suitable Node based on:

Resource requirements (CPU, memory)

Node taints/tolerations

Node affinity rules

Pod priorities

💡 29. What is a NodePort service range?

Answer:
By default, NodePort services use ports 30000–32767.
You can change this range in the kube-apiserver configuration.

🔄 30. What is the difference between kubectl apply, create, and replace?
Command	Description
kubectl create	Creates a new resource
kubectl apply	Creates or updates a resource declaratively
kubectl replace	Replaces an existing resource completely
📋 31. How do you check which node a Pod is running on?
kubectl get pod <pod-name> -o wide

🧱 32. What is Node Affinity and Anti-Affinity?

Answer:
These define where Pods should or shouldn’t run:

Affinity: Prefer running Pods together (e.g., same app tier)

Anti-Affinity: Avoid placing Pods together (e.g., for HA)

🚫 33. What are Taints and Tolerations?

Answer:
Used to control which Pods can be scheduled on which Nodes.

Taint: Applied on Node (repels Pods)

Toleration: Applied on Pod (allows it to tolerate certain taints)

🧠 Example:

kubectl taint nodes node1 key=value:NoSchedule

💾 34. How does Kubernetes handle container storage?

Answer:
Through Volumes, Persistent Volumes (PV), and Persistent Volume Claims (PVC).
They allow data persistence even after Pods are deleted.

🧰 35. What is a Sidecar container?

Answer:
A container that runs alongside the main app container to provide support functionality (e.g., logging, monitoring, proxying).

Example: NGINX reverse proxy or Datadog agent inside a Pod.

🧮 36. How do you check Pod resource usage (CPU/Memory)?
kubectl top pod
kubectl top node


(Requires metrics-server to be installed.)

🧱 37. What is Resource Requests and Limits?

Answer:

resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "200m"
    memory: "256Mi"


Requests → minimum guaranteed resources

Limits → maximum allowed resources

Helps the scheduler and prevents resource starvation.

🧩 38. What is Horizontal Pod Autoscaler (HPA)?

Answer:
Automatically scales the number of Pods based on CPU or memory utilization.

kubectl autoscale deployment myapp --min=2 --max=5 --cpu-percent=70

🧠 39. What is a Vertical Pod Autoscaler (VPA)?

Answer:
Adjusts resource requests/limits (CPU/memory) of Pods automatically, instead of changing Pod count.

🏗️ 40. What is Cluster Autoscaler?

Answer:
Automatically adds or removes nodes from a cluster based on pending Pods or low usage — mostly used in cloud environments (AWS, GCP, Azure).

🔄 41. How do you update a running Deployment image?
kubectl set image deployment/myapp myapp=nginx:latest
kubectl rollout status deployment/myapp

🕵️ 42. How to rollback a failed Deployment update?
kubectl rollout undo deployment/myapp

🧰 43. What is kube-proxy and its role?

Answer:
kube-proxy maintains network rules on nodes to allow network communication to your Pods (via Services).

🧩 44. What is the difference between ClusterIP and Headless Service?
ClusterIP	Headless Service
Provides a single virtual IP	No cluster IP assigned
Used for load-balancing	Used for direct Pod-to-Pod communication (stateful apps)
🔒 45. What is RBAC in Kubernetes?

Answer:
Role-Based Access Control manages user permissions in the cluster using:

Role/ClusterRole → defines permissions

RoleBinding/ClusterRoleBinding → assigns roles to users/service accounts

🧑‍💼 46. What is a ServiceAccount?

Answer:
A special account used by Pods to interact with the API server securely.
Useful for automation or monitoring tools (e.g., Datadog, Jenkins agents).

⚙️ 47. What is a Kubernetes Context?

Answer:
A context is a combination of cluster, user, and namespace configuration in your kubeconfig file.
You can switch between contexts:

kubectl config use-context <context-name>

🌍 48. What is a Custom Resource Definition (CRD)?

Answer:
CRD allows you to extend Kubernetes by defining your own custom resources (e.g., MySQLCluster, KafkaTopic).

🧩 49. What is a Pod Disruption Budget (PDB)?

Answer:
It ensures a minimum number of Pods stay running during voluntary disruptions (e.g., node maintenance).

minAvailable: 2

🔁 50. How do you perform zero downtime deployment?

Answer:
Use rolling updates via Deployments and readiness probes to ensure new Pods are ready before terminating old ones.

⚠️ 51. What are Liveness and Readiness Probes?

Answer:
They check the health of containers:

livenessProbe:
  httpGet:
    path: /health
    port: 8080
readinessProbe:
  httpGet:
    path: /ready
    port: 8080


Liveness: Restarts unhealthy containers

Readiness: Ensures Pod is ready to serve traffic

🧠 52. What happens when a Pod is terminated?

Answer:

Pod enters Terminating state

PreStop hooks run (if defined)

Containers receive SIGTERM signal

Grace period (default 30s)

Force kill (SIGKILL) if not exited

🧩 53. What is the default namespace in Kubernetes?

Answer:
default namespace — used when no namespace is specified.

💾 54. How do you take a backup of etcd?
etcdctl snapshot save backup.db
etcdctl snapshot restore backup.db

🧰 55. What is a Kubernetes Operator?

Answer:
An Operator extends K8s to automate complex application management (e.g., backups, upgrades) — built using CRDs + custom controllers.

🧭 Quick Recap

✅ Basic to Advanced Topics Covered:

Core components

Scheduling, autoscaling

Networking & storage

Security (RBAC, ServiceAccounts)

Probes, rolling updates, Operators

⚙️ 56. What is the difference between ReplicaSet and Deployment?

Answer:

ReplicaSet: Ensures the desired number of Pods are running.

Deployment: Manages ReplicaSets and enables rolling updates, rollbacks, and version control.

So → Deployment = ReplicaSet + rollout management.

🧩 57. How can you view all the resources in a namespace?
kubectl get all -n <namespace>

🧱 58. What happens when you delete a namespace?

Answer:
All resources inside that namespace (Pods, Services, Deployments, etc.) are also deleted.
⚠️ Be careful — it’s irreversible.

🧠 59. What is the difference between ConfigMap and Secret?
Feature	ConfigMap	Secret
Purpose	Stores configuration data	Stores sensitive data
Encoding	Plain text	Base64 encoded
Security	Less secure	More secure
🔒 60. How do you create a Secret from a file?
kubectl create secret generic mysecret --from-file=./password.txt


View decoded value:

kubectl get secret mysecret -o yaml
echo "<encoded>" | base64 --decode

⚙️ 61. How can you mount a ConfigMap into a Pod?
volumes:
  - name: config-volume
    configMap:
      name: app-config
volumeMounts:
  - mountPath: /etc/config
    name: config-volume

🧩 62. What is the purpose of initContainers?

Answer:
initContainers run before main containers — used for setup tasks like:

Cloning code

Checking dependencies

Setting up config files

💡 63. How do you check Pod restart count?
kubectl get pods -o wide
# OR
kubectl describe pod <pod-name> | grep -i restart

🧱 64. What is the difference between kubectl exec and kubectl attach?
Command	Use
kubectl exec	Run new commands in a running container
kubectl attach	Attach to the main process (stdout/stderr) of the container
🧰 65. How do you copy files between local and Pod?
kubectl cp <local-file> <namespace>/<pod-name>:/path/
kubectl cp <namespace>/<pod-name>:/path/ <local-dir>

⚙️ 66. What is a DaemonSet?

Answer:
Ensures one Pod runs on every Node (or a subset of nodes).
Example: log collectors, monitoring agents (Datadog, Fluentd, Node Exporter).

🧩 67. What is a StatefulSet?

Answer:
Manages stateful applications (like MySQL, MongoDB).
It maintains:

Persistent identity

Stable network name

Ordered startup/shutdown

💾 68. How can you debug a CrashLoopBackOff Pod?

Steps:

Describe the Pod → kubectl describe pod <name>

Check logs → kubectl logs <pod> --previous

Edit Deployment to run sleep command → debug manually

Check resource limits and probes

🧱 69. What is the difference between Liveness, Readiness, and Startup probes?
Probe	Purpose
Liveness	Restarts unhealthy containers
Readiness	Waits until app is ready to serve traffic
Startup	Waits for slow-starting apps before enabling liveness probe
🌐 70. What is Ingress in Kubernetes?

Answer:
Ingress provides HTTP/HTTPS routing to internal Services using path-based or host-based rules.

Example:

path: /app
service: myapp-service

🌍 71. What’s the difference between Ingress and LoadBalancer?
LoadBalancer	Ingress
Exposes one service per external IP	Routes multiple services under one IP
L4 (TCP/UDP)	L7 (HTTP/HTTPS)
Simple	Flexible (path/host rules)
🧩 72. How do you install NGINX Ingress Controller?
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl get pods -n ingress-nginx

⚙️ 73. How do you check Service endpoints?
kubectl get endpoints <service-name>

🧰 74. How do you scale a Deployment manually?
kubectl scale deployment <name> --replicas=5

🧠 75. What is a Pod template hash in ReplicaSet?

Answer:
Kubernetes generates a hash of the Pod template to differentiate ReplicaSets created during rolling updates.

💾 76. How can you store logs persistently in Kubernetes?

Mount hostPath or PersistentVolume to /var/log

Use a logging agent like Fluentd/Datadog/Promtail to collect logs

Push logs to S3, Elasticsearch, or CloudWatch

🧱 77. What’s the purpose of a Headless Service with StatefulSets?

Answer:
Each Pod gets a stable DNS name (e.g., mysql-0.mysql.default.svc.cluster.local) — used for inter-pod communication.

🔒 78. How do you restrict user access in Kubernetes?

Use RBAC (Role-Based Access Control):

Create Role → define permissions

Create RoleBinding → assign Role to user/service account

⚙️ 79. What is Helm in Kubernetes?

Answer:
Helm is a package manager for Kubernetes.

Packages = Charts

Simplifies deployment using templates and version control

Example:

helm install myapp ./mychart

🧩 80. What are advantages of using Helm?

✅ Versioned deployments
✅ Easy rollbacks
✅ Parameterized configuration (values.yaml)
✅ Reusable across environments (dev/stage/prod)
✅ Automates complex app setups (like Jenkins, Prometheus, Datadog)

🚀 Summary of All Sets
Set	Topics	Level
Set 1 (Q1–25)	Basics, Pods, Deployments, Services	Beginner
Set 2 (Q26–55)	Scheduling, Autoscaling, RBAC, Probes	Intermediate
Set 3 (Q56–80)	Helm, Ingress, StatefulSets, Troubleshooting	Advanced/Practical

🧱 81. What are Namespaces used for in Kubernetes?

Answer:
Namespaces logically divide a cluster into multiple virtual clusters.
They help in:

Multi-team or environment separation

Resource isolation

Role-based access control (RBAC) boundaries

Resource quotas per team or environment

⚙️ 82. How do you limit resource usage within a namespace?

Use ResourceQuota and LimitRange objects.

apiVersion: v1
kind: ResourceQuota
metadata:
  name: team-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "2"
    limits.memory: "4Gi"

🔒 83. How can you secure secrets at rest in Kubernetes?

Use encryption at rest (API server flag --encryption-provider-config)

Store secrets in external vaults like AWS KMS, HashiCorp Vault, or Sealed Secrets.

🧩 84. What are Sealed Secrets?

A Bitnami controller + CRD that allows storing encrypted secrets safely in Git repositories (GitOps-friendly).
You encrypt once → commit → decrypts automatically when applied to the cluster.

🧰 85. How do you run a one-time job in Kubernetes?

Use a Job:

apiVersion: batch/v1
kind: Job
metadata:
  name: db-migrate
spec:
  template:
    spec:
      containers:
        - name: migrate
          image: myapp:migrate
      restartPolicy: OnFailure

💡 86. Difference between a Job and a CronJob?
Job	CronJob
Runs once	Runs periodically (via schedule)
Immediate execution	Uses cron syntax (e.g., */5 * * * *)
🌐 87. How do you debug DNS issues in a Pod?

kubectl exec -it <pod> -- nslookup kubernetes.default

Check the CoreDNS Pod logs:

kubectl logs -n kube-system -l k8s-app=kube-dns


Verify DNS service IP from /etc/resolv.conf.

🧱 88. What is NetworkPolicy in Kubernetes?

Answer:
A way to control inbound/outbound traffic to Pods.
Example:

ingress:
  - from:
      - podSelector:
          matchLabels:
            app: frontend

🔒 89. By default, are Pods allowed to communicate with each other?

Yes.
All Pods within a cluster can talk to each other by default (flat networking model).
NetworkPolicies are used to restrict this.

⚙️ 90. What are the main components of the CNI (Container Network Interface)?

CNI plugin (e.g., Flannel, Calico, Weave)

Kubelet (calls the plugin)

CNI configuration files in /etc/cni/net.d

🧩 91. What’s the difference between Flannel and Calico?
Flannel	Calico
Simple overlay network (VXLAN)	Layer 3 network with network policies
Easier setup	More advanced (security, routing)
Less control	Supports BGP, eBPF
💾 92. How do you persist data in Kubernetes?

Use PersistentVolume (PV) for actual storage

Use PersistentVolumeClaim (PVC) to request storage

Mount the PVC in Pods under /data, /var/lib/... etc.

🧱 93. How do you check which Pod is consuming high resources?
kubectl top pod -A
kubectl describe pod <pod>

🧰 94. What happens if etcd is down?

Cluster becomes read-only.

You cannot create/update/delete resources.

Running Pods keep working, but new scheduling stops.

💡 95. How do you restart a Pod without deleting the Deployment?
kubectl rollout restart deployment <deployment-name>

⚙️ 96. How do you perform blue-green deployment in Kubernetes?

Create two deployments (blue & green)

Expose both via a single Service

Switch service selector between app=blue → app=green after verification.

🧩 97. How can you integrate Kubernetes with Jenkins?

Install Kubernetes plugin in Jenkins.

Use Jenkins agents as Pods (dynamic pod templates).

Run builds inside Kubernetes cluster for scalability.

🧠 98. How to deploy an app using Helm in Jenkins pipeline?
sh "helm upgrade --install myapp ./charts/myapp -n prod --values values-prod.yaml"

🌍 99. How to enable SSL/TLS termination in Kubernetes?

Use Ingress with TLS:

tls:
  - hosts:
      - myapp.com
    secretName: tls-secret


The tls-secret stores your certificate and key.

🧱 100. How can you monitor a Kubernetes cluster?

Metrics: Prometheus, Datadog, CloudWatch, Lens

Logs: Fluentd, Loki, Datadog, ELK

Dashboards: Grafana or Datadog dashboards

🧩 101. How do you export cluster configuration for backup?
kubectl get all --all-namespaces -o yaml > cluster-backup.yaml
etcdctl snapshot save backup.db

⚙️ 102. What is a Pod eviction?

Answer:
When a Pod is forcibly terminated because:

Node is under pressure (CPU/memory/disk)

Taints like node.kubernetes.io/disk-pressure applied
Kubernetes reschedules Pod on another healthy node.

🧠 103. How can you drain a node safely for maintenance?
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data
kubectl uncordon <node>

🔒 104. How to prevent Pods from being scheduled on a specific Node?
kubectl taint node <node> key=value:NoSchedule


Only Pods with matching tolerations will be scheduled there.

🧰 105. How can you check all events in a cluster for troubleshooting?
kubectl get events -A --sort-by=.metadata.creationTimestamp

⚡ Quick Recap of All 4 Sets
Set	Focus Area	Level
Set 1 (Q1–25)	Core Basics, Pods, Deployments, Services	🟢 Beginner
Set 2 (Q26–55)	Scheduling, Autoscaling, RBAC, Probes	🟡 Intermediate
Set 3 (Q56–80)	Helm, StatefulSets, Logging, Ingress	🟠 Advanced
Set 4 (Q81–105)	Security, Networking, CI/CD, Troubleshooting	🔴 Expert

🧩 Kubernetes Interview Questions (Set 5 – Advanced & Practical)
1. What is a ReplicaSet in Kubernetes?

A ReplicaSet ensures that a specified number of identical pod replicas are running at any time.
👉 Example: If one pod crashes, ReplicaSet automatically creates a new one.

2. How is ReplicaSet different from Deployment?

ReplicaSet maintains a set of identical pods.

Deployment provides declarative updates and manages ReplicaSets for rolling updates and rollbacks.

3. What is a StatefulSet and when do we use it?

A StatefulSet manages pods that require persistent identity and stable storage — like databases (MySQL, MongoDB).
It maintains fixed names and storage for each pod (e.g., mysql-0, mysql-1).

4. Explain DaemonSet and its use case.

A DaemonSet ensures that a copy of a pod runs on every node (or specific nodes).
👉 Example: Running Datadog agent, Fluentd, or Node Exporter on all nodes.

5. What is a CronJob in Kubernetes?

A CronJob runs tasks on a scheduled basis, similar to Linux cron.
Example:

schedule: "*/5 * * * *"


Runs a pod every 5 minutes.

6. What happens if a CronJob’s pod fails?

The Job controller retries based on backoffLimit and restartPolicy.
If retries exceed the limit, it marks the job as failed.

7. How do you scale a Deployment manually and automatically?

Manual scaling:
kubectl scale deployment nginx --replicas=3

Auto scaling:
Using HorizontalPodAutoscaler (HPA).

8. What is Horizontal Pod Autoscaler (HPA)?

HPA automatically adjusts the number of pods based on CPU/memory utilization.
It uses metrics-server for real-time monitoring.

9. What is a Vertical Pod Autoscaler (VPA)?

VPA automatically adjusts CPU and memory requests/limits for pods — useful for resource optimization.

10. Explain Cluster Autoscaler.

Cluster Autoscaler automatically adds/removes nodes from the cluster based on pending pods and resource usage — typically used in cloud providers like AWS or GCP.

11. What is a ConfigMap?

A ConfigMap is used to store non-sensitive configuration data (like environment variables, URLs).
Pods can consume them as environment variables or mounted files.

12. What is a Secret and how is it different from ConfigMap?

A Secret stores sensitive data (passwords, tokens) in base64 encoded format.
Unlike ConfigMap, it should be mounted with limited access permissions.

13. How do you create and use a Secret in a pod?
kubectl create secret generic my-secret --from-literal=password=admin123


Mount in pod:

env:
- name: PASSWORD
  valueFrom:
    secretKeyRef:
      name: my-secret
      key: password

14. What is the difference between kubectl apply, create, and replace?

create → Creates new resource.

apply → Creates or updates existing resource (declarative).

replace → Deletes and recreates resource.

15. What is kubectl exec used for?

Used to execute commands inside a running pod.

kubectl exec -it nginx-pod -- bash

16. What is the role of Kubernetes Scheduler?

The Scheduler assigns pods to nodes based on resource availability, taints/tolerations, and affinity rules.

17. How can you restrict pods from running on specific nodes?

Using nodeSelector

Using node affinity

Using taints and tolerations

18. What are taints and tolerations?

They control which pods can be scheduled on which nodes.
👉 Example:

kubectl taint nodes node1 key=value:NoSchedule


Only pods with matching tolerations will run there.

19. What is a readiness probe?

It checks if a pod is ready to serve traffic.
If not ready, the Service will not route traffic to that pod.

20. What is a liveness probe?

It checks if the application inside a pod is still running and healthy.
If the check fails, Kubernetes restarts the pod.

21. What is an init container?

A special container that runs before the main app container starts — used for setup or configuration.

22. What is the role of the kubelet?

Kubelet runs on every node, communicates with the API server, and ensures that the containers in the pod are running as expected.

23. What is the kube-proxy?

kube-proxy manages network rules and load balances traffic between pods and services on each node.

24. How do you view events in a namespace?
kubectl get events -n <namespace>


or

kubectl describe pod <pod-name>

25. What is the purpose of labels and selectors in Kubernetes?

Labels are key/value pairs attached to objects; selectors are used to filter or group resources based on labels.

⚙️ Kubernetes Interview Questions (Set 6 – Networking, Security & Troubleshooting)
1. What is a Kubernetes Service and why is it needed?

A Service exposes a set of pods as a single network endpoint.
Since pods are ephemeral (their IPs change), Services ensure stable access to them.

2. What are the types of Kubernetes Services?

ClusterIP – Default, accessible only within cluster

NodePort – Exposes service on each node’s IP (30000–32767 range)

LoadBalancer – Uses external load balancer (in cloud)

ExternalName – Maps service to an external DNS name

3. What is kube-proxy’s role in networking?

kube-proxy maintains network rules and load balances traffic between pods across nodes, implementing Service IPs using iptables or IPVS.

4. How does DNS work in Kubernetes?

Kubernetes uses CoreDNS for internal name resolution.
Pods can resolve services via <service-name>.<namespace>.svc.cluster.local.

5. What is a NetworkPolicy in Kubernetes?

NetworkPolicies control which pods can communicate with each other and with external systems — like a firewall for pods.
They work with supported CNI plugins (Calico, Cilium, etc).

6. Give an example of NetworkPolicy.
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
    - from:
        - podSelector:
            matchLabels:
              access: allowed


Only pods with access=allowed can reach nginx.

7. What is the difference between Ingress and Service?

Service exposes pods to cluster/internal/external traffic.

Ingress provides HTTP/HTTPS routing at Layer 7 using rules and host/path-based routing.

8. What is an Ingress Controller?

An Ingress Controller (like NGINX, Traefik, HAProxy) watches Ingress resources and implements routing rules dynamically.

9. What is the default namespace in Kubernetes?

By default, resources go into the default namespace if not specified.

10. How do you list resources from all namespaces?
kubectl get all --all-namespaces

11. How do you debug a pod stuck in CrashLoopBackOff?

Steps:

Check logs → kubectl logs pod-name

Describe pod → kubectl describe pod pod-name

Look for failing liveness probes, config errors, or missing volumes.

12. What causes an ImagePullBackOff error?

Common causes:

Wrong image name/tag

Missing image pull secret (private registry)

No internet access to registry

Fix:

kubectl create secret docker-registry mysecret --docker-username=... --docker-password=...

13. How do you check which node a pod is running on?
kubectl get pod -o wide


It shows the node name under the NODE column.

14. How do you drain a node for maintenance?
kubectl drain node-name --ignore-daemonsets --delete-emptydir-data


After maintenance:

kubectl uncordon node-name

15. What is a PersistentVolume (PV)?

PV is a piece of storage in the cluster (like EBS, NFS) provisioned by an admin.

16. What is a PersistentVolumeClaim (PVC)?

PVC is a request for storage by a user/pod.
It binds to a PV with matching size and access mode.

17. Explain StorageClass.

StorageClass defines dynamic storage provisioning.
It automates volume creation using cloud providers or plugins.

18. What is a sidecar container?

A helper container that runs alongside the main container in a pod — e.g., for logging, monitoring, or proxying.

19. What is the purpose of kubectl describe?

Gives detailed info about a resource — status, events, conditions — useful for debugging.

20. How do you view cluster events in real time?
kubectl get events -w


(-w = watch mode)

21. What is Pod Disruption Budget (PDB)?

PDB ensures minimum number of pods remain available during voluntary disruptions (like node upgrades).

Example:

minAvailable: 2

22. How can you limit CPU and memory usage in Kubernetes?

Using Resource Quotas and LimitRanges at namespace level.

23. What is Role-Based Access Control (RBAC)?

RBAC restricts actions users or services can perform.
It includes:

Role / ClusterRole

RoleBinding / ClusterRoleBinding

24. What are Admission Controllers?

They intercept requests to the API server before persistence, enforcing policies like resource limits, security, or image whitelisting.

25. How do you upgrade a Kubernetes deployment with zero downtime?

Use rolling updates:

kubectl set image deployment/nginx nginx=nginx:1.27


and rollback if needed:

kubectl rollout undo deployment/nginx

🧠 Kubernetes Interview Questions (Set 7 – Real-World Scenarios & DevOps Focus)
1. How do you use ConfigMap in a Deployment?

ConfigMap stores configuration data (non-sensitive).

Mount it as environment variables or files inside pods.

Example:

envFrom:
- configMapRef:
    name: app-config

2. How do you update ConfigMap without restarting pods?

Mount as a volume; updates are reflected inside pod automatically (if using volume mount).

For env vars, pods need to be restarted.

3. How do you create a Deployment that always pulls the latest image?
imagePullPolicy: Always

4. Difference between RollingUpdate and Recreate strategy?

RollingUpdate → Updates pods gradually with zero downtime.

Recreate → Deletes all pods first, then creates new ones (downtime).

5. How do you rollback a Deployment?
kubectl rollout undo deployment <deployment-name>

6. How can you check rollout status?
kubectl rollout status deployment/<deployment-name>

7. What is a headless service and when is it used?

No cluster IP.

Directs traffic to pods individually.

Used with StatefulSets for stable DNS names.

8. How do you enable logging for NGINX in Kubernetes?

Mount hostPath or PersistentVolume to /var/log/nginx.

Access logs using:

kubectl exec -it <nginx-pod> -- cat /var/log/nginx/access.log

9. How do you run a one-off command in a pod?
kubectl run tmp-pod --rm -i --tty --image=busybox -- /bin/sh

10. How do you forward a port from a pod to your local machine?
kubectl port-forward pod/<pod-name> 8080:80

11. How do you forward a Service port?
kubectl port-forward service/<service-name> 8080:80

12. What is the difference between kubectl logs and kubectl attach?

logs → View past and current logs

attach → Attach to a live process’s stdout/stderr

13. How do you get previous logs of a crashing pod?
kubectl logs <pod-name> --previous

14. What is a sidecar pattern?

A secondary container in the same pod, usually for logging, monitoring, or proxying.

Example: Fluentd sidecar collecting logs from app container.

15. How do you define resource requests and limits?
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"


Requests → Reserved resources

Limits → Maximum allowed

16. How do you prevent pods from using too much CPU or memory?

Set limits in pod spec

Use LimitRange at namespace level

17. How do you check if the metrics-server is running?
kubectl get deployment metrics-server -n kube-system

18. How do you enable HPA (Horizontal Pod Autoscaler)?
kubectl autoscale deployment nginx --cpu-percent=50 --min=2 --max=5

19. What is the difference between persistent volume (PV) and ephemeral storage?

PV → Persistent storage, survives pod restart

Ephemeral → Stored in container’s filesystem, lost if pod dies

20. How do you mount a PVC in a pod?
volumes:
  - name: my-volume
    persistentVolumeClaim:
      claimName: my-pvc
volumeMounts:
  - mountPath: /data
    name: my-volume

21. What is the purpose of kubectl describe vs kubectl get?

get → Lists resources, brief info

describe → Detailed info including events, status, and conditions

22. How do you check node resource usage?
kubectl top nodes

23. How do you check pod resource usage?
kubectl top pods -n <namespace>

24. How do you debug a pod stuck in Pending?

Check:

kubectl describe pod <pod-name>


Common causes:

Insufficient resources

NodeSelector mismatch

PVC not bound

25. How do you test network connectivity between pods?
kubectl exec -it <pod1> -- ping <pod2-ip>
kubectl exec -it <pod1> -- curl http://<pod2-service>:<port>


This set focuses on DevOps practical scenarios — logs, probes, autoscaling, networking, PVC, HPA, debugging, and port-forwarding — exactly what you’ll face in real interviews.

If you want, I can compile all 155+ Kubernetes questions (Sets 1–7) into a full “Kubernetes DevOps Interview Handbook 2025” in PDF/Markdown with:

Commands cheat sheet

YAML examples

Scenarios & troubleshooting tips
