# ⚙️ Kubernetes Jobs & CronJobs — Complete Guide

## 🧭 Overview

Kubernetes **Jobs** and **CronJobs** are part of the **Batch API** used for running short-lived, one-time, or scheduled tasks.

| Type        | Description                                                    | Example Use Case                                           |
| ----------- | -------------------------------------------------------------- | ---------------------------------------------------------- |
| **Job**     | Executes a task **once** and ensures it completes successfully | Database migration, file processing, one-time data cleanup |
| **CronJob** | Executes a task **on a schedule** (like a Linux cron)          | Daily backups, periodic report generation, system updates  |

---

## 🧱 What is a Job?

A **Job** creates one or more **Pods** to perform a task until completion.
When the task finishes successfully, the Job is marked as **completed**.

> 🧩 Example: Running a one-time script, performing data import/export, etc.

---

## ✍️ Job YAML Example

### **job.yml**

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: demo-job
  namespace: nginx
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 4
  template:
    metadata:
      name: demo-job-pod
      labels:
        app: batch-task
    spec:
      containers:
        - name: batch-container
          image: busybox:latest
          command: ["sh", "-c", "echo Hello Dosto && sleep 10"]
      restartPolicy: Never
```

### **Commands**

```bash
# Apply job
kubectl apply -f job.yml

# List jobs
kubectl get jobs -n nginx

# Get pods created by job
kubectl get pods -n nginx

# Describe job
kubectl describe job demo-job -n nginx

# Check job logs
kubectl logs <pod-name> -n nginx

# Delete job
kubectl delete -f job.yml
# ⚠️ Important: delete old job before reapplying a new one with same name
```

---

## ⚙️ Important Job Fields

| Field                   | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| `completions`           | Number of successful Pods required to mark the Job as complete      |
| `parallelism`           | Number of Pods that can run simultaneously                          |
| `backoffLimit`          | Number of retries before marking the Job as failed                  |
| `activeDeadlineSeconds` | Maximum duration (in seconds) a Job can run before being terminated |
| `restartPolicy`         | Should be `Never` or `OnFailure` (required for Jobs)                |

---

## 🔍 Useful Job Commands

```bash
# View all jobs and their status
kubectl get jobs -A

# Check Job details
kubectl describe job <job-name> -n <namespace>

# Get pod logs from Job
kubectl logs -f <pod-name> -n <namespace>

# Get pods related to specific job
kubectl get pods --selector=job-name=demo-job -n nginx
```

---

## 🚀 What is a CronJob?

A **CronJob** runs a **Job** on a recurring schedule — similar to Linux cron syntax.
Useful for tasks like backups, report generation, periodic cleanup, etc.

---

## ✍️ CronJob YAML Example

### **cronjob.yml**

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: minute-backup
  namespace: nginx
spec:
  schedule: "* * * * *"  # Every minute
  jobTemplate:
    spec:
      template:
        metadata:
          name: minute-backup
          labels:
            app: minute-backup
        spec:
          containers:
            - name: backup-container
              image: busybox
              command:
                - sh
                - -c
                - >
                  echo "Backup started" && \
                  mkdir -p /backups && \
                  mkdir -p /demo-data && \
                  cp -r /demo-data /backups && \
                  echo "Backup completed";
              volumeMounts:
                - name: data-volume
                  mountPath: /demo-data
                - name: backup-data
                  mountPath: /backups
          restartPolicy: OnFailure
          volumes:
            - name: data-volume
              hostPath:
                path: /demo-data
                type: DirectoryOrCreate
            - name: backup-data
              hostPath:
                path: /backups
                type: DirectoryOrCreate
```

### **Commands**

```bash
# Apply cronjob
kubectl apply -f cronjob.yml

# List all cronjobs
kubectl get cronjob -n nginx

# List all pods created by cronjob
kubectl get pods -n nginx

# Delete cronjob
kubectl delete -f cronjob.yml
```

---

## 🧮 Cron Schedule Syntax (⭐ 5 Fields)

| Field       | Description            | Example        |
| ----------- | ---------------------- | -------------- |
| `*`         | Every minute (0-59)    | `* * * * *`    |
| `*/5`       | Every 5 minutes        | `*/5 * * * *`  |
| `0`         | Every hour at minute 0 | `0 * * * *`    |
| `0 0 * * *` | Every day at midnight  | Daily backups  |
| `0 3 * * 0` | Every Sunday at 3 AM   | Weekly cleanup |

> 🔗 Tip: Use [crontab.guru](https://crontab.guru) to easily design schedules.

---

## ⚙️ Important CronJob Fields

| Field                        | Description                                             |
| ---------------------------- | ------------------------------------------------------- |
| `schedule`                   | Defines when the job runs (cron format)                 |
| `jobTemplate`                | Template defining the Job spec that runs on schedule    |
| `concurrencyPolicy`          | Prevents overlapping runs: `Allow`, `Forbid`, `Replace` |
| `startingDeadlineSeconds`    | Deadline to start a missed job run                      |
| `successfulJobsHistoryLimit` | How many completed jobs to retain                       |
| `failedJobsHistoryLimit`     | How many failed jobs to retain                          |

---

## 🧠 Practical Example Summary

```bash
# Create a namespace
kubectl create ns nginx

# Create a Job
kubectl apply -f job.yml

# Get logs from Job Pod
kubectl logs <job-pod> -n nginx

# Create a CronJob (runs every minute)
kubectl apply -f cronjob.yml

# Watch CronJob executions
kubectl get pods -w -n nginx

# Delete both
kubectl delete -f job.yml -f cronjob.yml
```

---

## 🧾 Summary

| Concept             | Description                                          |
| ------------------- | ---------------------------------------------------- |
| **Job**             | Runs a Pod until task completion                     |
| **CronJob**         | Runs a Job on a defined schedule                     |
| **Completions**     | Total Pods that must complete for the Job to succeed |
| **Parallelism**     | Number of Pods running at a time                     |
| **BackoffLimit**    | Retry count before failing the Job                   |
| **RestartPolicy**   | `Never` or `OnFailure` only                          |
| **Schedule Format** | `* * * * *` (minute hour day month weekday)          |

---

✅ **In short:**

> **Jobs** are for one-time tasks, **CronJobs** are for recurring tasks.
> Together, they help automate workloads like **backups, cleanup, data processing, and monitoring scripts** in Kubernetes clusters.
