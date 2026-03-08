# 23 - Monitoring

## 1. Introduction

Monitoring is essential in Kubernetes to track the health, performance, and resource usage of applications and cluster components.

In production environments, monitoring helps detect:

* Application failures
* Resource bottlenecks
* Node health issues
* Performance degradation

Monitoring allows DevOps teams to maintain **high availability and reliability**.

---

# 2. What Needs to Be Monitored

In a Kubernetes cluster, multiple layers need monitoring.

Important components include:

Pods

Nodes

Containers

Cluster components

Network traffic

Application performance

Monitoring these components helps identify problems quickly.

---

# 3. Types of Monitoring

There are several types of monitoring in Kubernetes.

### Infrastructure Monitoring

Tracks node health and hardware resource usage.

Examples:

CPU usage

Memory usage

Disk usage

### Application Monitoring

Tracks application behavior.

Examples:

Response time

Error rates

Request counts

### Cluster Monitoring

Tracks Kubernetes components such as:

API Server

Scheduler

Controller Manager

---

# 4. Kubernetes Metrics

Kubernetes collects various metrics including:

CPU usage

Memory usage

Network usage

Disk usage

Pod restart count

Node health status

These metrics help administrators understand cluster performance.

---

# 5. Metrics Server

Metrics Server is a lightweight monitoring component in Kubernetes.

It collects resource usage metrics from kubelets.

Example metrics collected:

CPU usage

Memory usage

Metrics Server enables commands such as:

kubectl top nodes

kubectl top pods

Example:

kubectl top pods

This shows CPU and memory usage of pods.

---

# 6. Prometheus

Prometheus is one of the most widely used monitoring tools for Kubernetes.

It collects time-series metrics and stores them in a database.

Prometheus features:

* Metrics collection
* Powerful query language (PromQL)
* Alerting system

Prometheus scrapes metrics from Kubernetes components.

Example metrics targets:

Pods

Nodes

API server

Applications

---

# 7. Grafana

Grafana is a visualization tool used to display metrics collected by Prometheus.

It provides dashboards for monitoring cluster performance.

Example dashboards:

Node resource usage

Pod CPU usage

Cluster health

Network traffic

Grafana helps teams visualize monitoring data in real time.

---

# 8. Prometheus + Grafana Architecture

Typical monitoring architecture:

Kubernetes Cluster

↓
Prometheus (metrics collection)

↓
Grafana (visualization dashboards)

Prometheus gathers metrics and Grafana displays them.

---

# 9. Alerting

Monitoring systems should also generate alerts.

Prometheus Alertmanager sends alerts when issues occur.

Examples:

High CPU usage

Node failure

Pod crash loops

Low disk space

Alerts can be sent to:

Email

Slack

PagerDuty

---

# 10. Logging vs Monitoring

Monitoring tracks metrics and system performance.

Logging records application events.

Example logging tools:

* ELK Stack
* Fluentd
* Loki

Monitoring + Logging together provide full observability.

---

# 11. Popular Kubernetes Monitoring Tools

Common monitoring tools include:

Prometheus

Grafana

Datadog

New Relic

Elastic Stack

Prometheus + Grafana is the most popular open-source combination.

---

# 12. Real World Example

Example monitoring stack:

Prometheus → collects metrics

Grafana → visualizes metrics

Alertmanager → sends alerts

This setup provides complete cluster monitoring.

---

# 13. Monitoring Best Practices

Recommended practices:

Monitor both infrastructure and applications

Use dashboards for visualization

Set alerts for critical metrics

Monitor pod restarts and resource usage

Proactive monitoring helps prevent production failures.

---

# 14. Debugging with Monitoring

Monitoring helps identify problems quickly.

Example scenario:

Application slow response.

Monitoring dashboard shows:

High CPU usage on pods.

Solution:

Scale pods or optimize application.

---

# 15. Important Interview Questions

### Why is monitoring important in Kubernetes?

Monitoring helps track cluster health, detect failures, and maintain application performance.

### What is Metrics Server?

Metrics Server collects resource usage metrics like CPU and memory from nodes and pods.

### Why are Prometheus and Grafana commonly used together?

Prometheus collects metrics and Grafana visualizes those metrics using dashboards.

---

# 16. Summary

Monitoring ensures the stability and performance of Kubernetes clusters.

Key components:

Metrics Server

Prometheus

Grafana

Alertmanager

A good monitoring setup provides visibility into cluster health and application performance.
