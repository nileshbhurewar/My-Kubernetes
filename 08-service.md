# 🌐 Kubernetes Service — Exposing NGINX to the Outside World

A **Service** in Kubernetes is an abstraction that defines a logical set of Pods and a policy by which to access them. It enables communication between different parts of your application and exposes your app internally or externally.

---

## ⚙️ Purpose of a Service

* It provides **stable networking** for Pods (which are ephemeral).
* It allows **external access** to applications running inside the cluster.
* It balances traffic across multiple replicas (Pods) of the same app.

---

## 🧩 Example: NGINX Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: nginx
spec:
  selector:
    app: nginx   # Matches pods with label app=nginx
  ports:
    - protocol: TCP
      port: 80        # Port exposed by the Service (external)
      targetPort: 80  # Port on which the container (Pod) listens
      nodePort: 30080 # Port used only with NodePort type (range: 30000–32767)
  type: NodePort      # Can be ClusterIP / NodePort / LoadBalancer
```

---

## 🧠 Service Types Explained

| Type             | Description                                           | Accessibility                       |
| ---------------- | ----------------------------------------------------- | ----------------------------------- |
| **ClusterIP**    | Default type, exposes service **within cluster only** | Internal only                       |
| **NodePort**     | Exposes service **outside cluster on a static port**  | Accessible via Node IP and NodePort |
| **LoadBalancer** | Creates an external load balancer (in cloud)          | Public internet access              |

Example access for NodePort:

```bash
http://<Node-IP>:30080
```

---

## 🚀 Commands

Apply the Service YAML:

```bash
kubectl apply -f service.yml
```

Check resources in the namespace:

```bash
kubectl get all -n nginx
```

---

## 🌍 Accessing the NGINX Page

If you’re using **Minikube** or running on **EC2**, you can use **port-forwarding** to expose the service temporarily.

```bash
kubectl port-forward service/nginx-service -n nginx 80:80 --address=0.0.0.0
```

If any permission errors occur:

```bash
sudo -E kubectl port-forward service/nginx-service -n nginx 80:80 --address=0.0.0.0
```

Now, open your EC2 Public IP in the browser:

```
http://<EC2-Public-IP>
```

You’ll see the default NGINX welcome page.

---

✅ **Summary:**

* Service connects external traffic to Pods.
* `ClusterIP` = internal only.
* `NodePort` = exposed via Node IP.
* `LoadBalancer` = cloud-managed external access.
* Use port-forwarding for local access in Minikube/EC2 setups.
