# AWS Cloud Native Retail Store Application

## Overview

This project demonstrates the deployment of a cloud-native Retail Store microservices application on a self-managed Kubernetes cluster running on AWS EC2.

The objective of this project was to gain hands-on experience with real-world DevOps practices including Infrastructure provisioning, Docker containerization, Kubernetes orchestration, Helm package management, Persistent Volumes, Ingress configuration, rolling deployments, and cloud networking.

The complete infrastructure was manually configured without using managed Kubernetes services like Amazon EKS.

---

# Architecture

```
                    Internet
                        |
                  AWS Security Group
                        |
                 AWS EC2 Instances
                        |
        ------------------------------------
        |                                  |
    Kubernetes Master                 Worker Nodes
        |                           /             \
        |                    Worker-1         Worker-2
        |
        |---------------- Kubernetes Cluster ----------------|
                          |
                    kube-apiserver
                          |
                    Kubernetes Services
                          |
                  Retail Store Application
                          |
 ----------------------------------------------------------
 | Catalog | Orders | Checkout | UI | DynamoDB Local DB |
 ----------------------------------------------------------

```

---

# Infrastructure

## AWS

- AWS EC2
- Ubuntu Server
- VPC
- Security Groups
- Elastic IP
- SSH

---

# Kubernetes Cluster

Cluster Type

- Self Managed Kubernetes Cluster

Version

```
v1.29.15
```

Nodes

| Node | Role |
|-------|------|
| k8s-master | Control Plane |
| k8s-worker-1 | Worker |
| k8s-worker-2 | Worker |

Container Runtime

```
containerd
```

Networking

```
Flannel CNI
```

---

# Technologies Used

- AWS EC2
- Ubuntu
- Docker
- Kubernetes
- Helm
- NGINX Ingress Controller
- Persistent Volume
- Persistent Volume Claim
- NodePort Service
- ClusterIP Service
- Java Spring Boot
- Maven
- DynamoDB Local

---

# Project Structure

```
retail-store-sample-app/

├── src/
│
├── catalog/
│
├── checkout/
│
├── orders/
│
├── ui/
│
├── carts-db/
│
├── Dockerfiles
│
├── Kubernetes YAML
│
└── Helm Charts
```

---

# Microservices

The application consists of the following services.

| Service | Purpose |
|----------|----------|
| UI | Frontend |
| Catalog | Product Catalog |
| Checkout | Order Checkout |
| Orders | Order Service |
| Carts DB | DynamoDB Local |

---

# Docker

Every microservice was containerized using Docker.

Images were built from their respective Dockerfiles.

Example

```
docker build -t ui:v1 .
```

---

# Kubernetes Objects Created

## Namespace

```
retail-store
```

---

## Deployments

- UI
- Catalog
- Checkout
- Orders
- Carts DB

---

## ReplicaSets

Automatically managed by Deployments.

---

## Pods

Successfully running

```
kubectl get pods -n retail-store
```

Output

```
9 Running Pods
```

---

## Services

Created

- ClusterIP
- NodePort

Example

```
kubectl get svc
```

```
catalog-service
orders-service
checkout-service
ui-service
carts-db
```

---

# Rolling Updates

Updated application images without downtime.

Verified using

```
kubectl rollout status deployment ui
```

Rollback command

```
kubectl rollout undo deployment ui
```

---

# Scaling

Scaled deployments manually.

Example

```
kubectl scale deployment ui --replicas=2
```

Verified

```
kubectl get deployment
```

---

# Health Verification

Pods

```
kubectl get pods
```

Deployments

```
kubectl get deployment
```

Services

```
kubectl get svc
```

ReplicaSets

```
kubectl get rs
```

---

# Helm

Installed

```
Helm v3
```

Used for

- Installing NGINX Ingress Controller
- Installing Prometheus Stack

---

# NGINX Ingress

Installed

```
ingress-nginx
```

Created

```
Ingress Resource
```

Host

```
retail.local
```

Application successfully routed through Ingress.

---

# Persistent Storage

Implemented

## Persistent Volume

```
2 Gi
```

## Persistent Volume Claim

Successfully bound

```
kubectl get pv

STATUS

Bound
```

```
kubectl get pvc

STATUS

Bound
```

Persistent storage attached to

```
carts-db
```

---

# Application Deployment

Successfully deployed

```
Catalog Service
Orders Service
Checkout Service
UI Service
Carts DB
```

Verified

```
kubectl get all -n retail-store
```

---

# Commands Used Frequently

Pods

```
kubectl get pods -n retail-store
```

Deployments

```
kubectl get deployment -n retail-store
```

Services

```
kubectl get svc -n retail-store
```

Logs

```
kubectl logs POD_NAME
```

Describe

```
kubectl describe pod POD_NAME
```

Rollout

```
kubectl rollout status deployment ui
```

Scaling

```
kubectl scale deployment ui --replicas=2
```

---

# Monitoring Attempt

Installed successfully

```
Prometheus
Grafana
Node Exporter
Alertmanager
Prometheus Operator
```

All monitoring pods were running successfully.

However, due to networking issues inside the Kubernetes cluster, the dashboards could not be accessed externally.

---

# HPA Attempt

Installed Metrics Server.

Encountered

```
Metrics API not available
```

Root Cause

Internal Kubernetes networking/DNS issue prevented Metrics Server from communicating with the API Server.

Therefore

Horizontal Pod Autoscaler could not be demonstrated.

---

# Challenges Faced

- Metrics Server API unavailable
- DNS communication issues inside cluster
- Admission Webhook timeout during Ingress creation
- Persistent Volume binding issues
- Service accessibility debugging
- NodePort accessibility troubleshooting

---

# Problems Solved

✔ Built Kubernetes Cluster manually

✔ Configured multi-node architecture

✔ Installed Container Runtime

✔ Configured Flannel networking

✔ Built Docker Images

✔ Deployed Microservices

✔ Created Services

✔ Created Deployments

✔ Managed ReplicaSets

✔ Configured Rolling Updates

✔ Configured Persistent Volumes

✔ Configured Persistent Volume Claims

✔ Installed Helm

✔ Installed NGINX Ingress

✔ Successfully routed application through Ingress

---

# Future Improvements

- Horizontal Pod Autoscaler
- Prometheus Monitoring
- Grafana Dashboard
- Blue-Green Deployment
- Canary Deployment
- CI/CD using Jenkins
- ArgoCD
- AWS ECR Integration
- TLS using Cert Manager
- Domain Name with Route53
- External Load Balancer

---

# Learning Outcomes

Through this project I gained practical experience in:

- Docker Containerization
- Kubernetes Administration
- Kubernetes Networking
- Helm Package Management
- Persistent Storage
- Ingress Management
- Rolling Updates
- Scaling Applications
- Cloud Infrastructure
- Linux Administration
- AWS EC2 Management
- Troubleshooting Kubernetes

---

# Project Status

| Feature | Status |
|----------|--------|
| AWS Infrastructure | ✅ Completed |
| Docker | ✅ Completed |
| Kubernetes Cluster | ✅ Completed |
| Multi Node Cluster | ✅ Completed |
| Microservices Deployment | ✅ Completed |
| Services | ✅ Completed |
| Rolling Updates | ✅ Completed |
| Scaling | ✅ Completed |
| Persistent Volume | ✅ Completed |
| Persistent Volume Claim | ✅ Completed |
| Helm | ✅ Completed |
| NGINX Ingress | ✅ Completed |
| Application Deployment | ✅ Completed |
| HPA | ❌ Pending |
| Metrics Server | ❌ Pending |
| Prometheus Dashboard | ❌ Pending |
| Grafana Dashboard | ❌ Pending |
| Blue-Green Deployment | ❌ Pending |

---

# Author

**Aftab Attar**

AWS Cloud & DevOps Engineer

GitHub: https://github.com/<your-username>

LinkedIn: https://linkedin.com/in/<your-profile>
