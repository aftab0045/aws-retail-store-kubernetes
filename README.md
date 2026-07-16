#  AWS Cloud Native Retail Store Application on Kubernetes

> A production-style Cloud & DevOps project demonstrating the deployment of a multi-tier microservices application on Amazon Web Services (AWS) using Docker, Kubernetes, Ingress, Persistent Volumes, and DevOps best practices.

---

#  Project Overview

This project demonstrates how to deploy a cloud-native microservices application on an AWS-based Kubernetes cluster. The application consists of multiple independent services that communicate with each other using Kubernetes networking. Each service is containerized with Docker and deployed as Kubernetes Deployments and Services. The complete infrastructure is hosted on Amazon EC2 instances running Ubuntu Server with Kubernetes installed using kubeadm.

The primary objective of this project was to gain hands-on experience with real-world DevOps practices such as containerization, orchestration, service discovery, networking, storage management, ingress routing, scaling architecture, and production deployment strategies.

Unlike a simple Docker deployment, this project focuses on deploying a complete distributed application similar to what organizations use in production environments.

---

#  Project Objectives

The main goals of this project were:

- Learn Kubernetes architecture from scratch.
- Deploy a complete microservices application.
- Containerize applications using Docker.
- Build and manage Kubernetes clusters.
- Configure networking between services.
- Implement Persistent Volumes.
- Configure Ingress Controller.
- Practice production deployment concepts.
- Understand real DevOps workflows.
- Prepare a production-ready portfolio project.

---

# Project Architecture

```
                    Internet
                        │
                AWS Security Group
                        │
                  Ingress Controller
                        │
                 Kubernetes Cluster
                        │
 ┌───────────────────────────────────────────────────────┐
 │                                                       │
 │                 Retail Store Namespace                │
 │                                                       │
 │  UI Service  →  Catalog Service  → Orders Service     │
 │                     │                 │               │
 │                     └──── Checkout ───┘               │
 │                                                       │
 │                 DynamoDB Local Database               │
 │                                                       │
 └───────────────────────────────────────────────────────┘

```

![](/Images/Screenshot%202026-07-15%20170547.png)

---

#  AWS Infrastructure

The complete infrastructure was deployed on AWS EC2.

The cluster consists of four Ubuntu Linux servers.

| Instance | Purpose |
|-----------|----------|
| Jenkins Server | CI/CD and Docker image building |
| Kubernetes Master | Cluster Management |
| Kubernetes Worker 1 | Application Workloads |
| Kubernetes Worker 2 | Application Workloads |

Each instance communicates over a private network inside AWS.

---

#  EC2 Configuration

All servers use:

- Ubuntu Server
- Containerd Runtime
- Kubernetes v1.29
- kubeadm
- kubelet
- kubectl

Networking between nodes is handled through the Kubernetes networking layer using Flannel CNI.

---

#  Kubernetes Cluster

The Kubernetes cluster was initialized using kubeadm.

The master node manages:

- Scheduling
- API Server
- Controller Manager
- etcd
- Cluster Administration

Worker nodes execute the application workloads.

The cluster successfully joins all worker nodes and provides a production-like Kubernetes environment.

---

# Application Architecture

The Retail Store application consists of independent microservices.

### UI

The UI service provides the frontend web interface where users interact with the application.

It communicates with backend services through Kubernetes ClusterIP Services.

---

### Catalog Service

The catalog service provides product information.

It exposes REST APIs consumed by the UI.

---

### Orders Service

The orders service manages customer order processing.

It communicates internally with checkout and database services.

---

### Checkout Service

Checkout handles order confirmation and purchasing workflow.

It communicates with the orders service.

---

### Carts Database

A local DynamoDB database stores cart information.

The database is deployed as its own Kubernetes Deployment.

Persistent storage is attached using Persistent Volumes.

---

# Docker Containerization

Every microservice is containerized independently.

Separate Dockerfiles are maintained for each service.

Each Docker image is built on the Jenkins server and later deployed to Kubernetes.

Example services:

- UI
- Catalog
- Checkout
- Orders

Containerization makes deployment portable and consistent across environments.

---

# Kubernetes Manifests

The project uses Kubernetes YAML manifests for every resource.

Resources include:

- Namespace
- Deployment
- Service
- Persistent Volume
- Persistent Volume Claim
- Ingress

Every microservice has its own Deployment and Service manifest.

This makes the project modular and easy to maintain.

---

# Namespace

A dedicated namespace named **retail-store** was created.

Using namespaces provides:

- Resource isolation
- Better organization
- Easier management
- Production-like deployment

All application components are deployed inside this namespace.

---

# Deployments

Each microservice is deployed using Kubernetes Deployments.

Deployments provide:

- Replica management
- Rolling updates
- Automatic recovery
- Pod recreation
- High availability

Multiple replicas were configured for frontend and backend services.

---

# Services

Each Deployment is exposed internally using Kubernetes Services.

The following Service types were used:

### ClusterIP

Used for internal communication between microservices.

Services include:

- Catalog
- Orders
- Checkout
- Database

---

### NodePort

The frontend UI is exposed using NodePort.

This allows external access to the application through the worker node IP address.

---

# Ingress Controller

NGINX Ingress Controller was installed using Helm.

An Ingress resource was configured to expose the application through a single entry point.

The controller successfully routed incoming HTTP traffic to the frontend service.

During setup, the admission webhook created networking issues that were resolved by changing the webhook failure policy.

---

# Persistent Storage

Persistent storage was implemented for the database.

The following Kubernetes resources were created:

- Persistent Volume (PV)
- Persistent Volume Claim (PVC)

The PV provides physical storage while the PVC requests storage for the application.

This ensures that application data survives Pod restarts.

---

# Replica Management

Multiple replicas were configured for important services.

Benefits include:

- High Availability
- Load Distribution
- Fault Tolerance
- Zero Downtime

Kubernetes automatically recreates failed Pods.

---

# Kubernetes Networking

Internal communication uses Kubernetes DNS.

Each service communicates using service names instead of IP addresses.

Example:

```
catalog-service
orders-service
checkout-service
```

This allows services to discover each other automatically.

---

# Security

Security was implemented using:

- Kubernetes Namespaces
- Private Cluster Networking
- AWS Security Groups
- Non-root Containers
- Internal ClusterIP Services

Only the frontend service is exposed externally.

---

# Scalability

The architecture is horizontally scalable.

New replicas can be created without modifying application code.

This provides:

- Better performance
- Improved availability
- Fault tolerance

---

# Testing

The deployment was verified using Kubernetes commands.

Examples:

```bash
kubectl get pods

kubectl get svc

kubectl get deployments

kubectl describe pod

kubectl logs
```

The application was successfully accessed from the browser using the NodePort service.

---

# DevOps Tools Used

- AWS EC2
- Ubuntu Linux
- Docker
- Kubernetes
- kubeadm
- kubectl
- Containerd
- Flannel CNI
- Helm
- NGINX Ingress Controller
- Persistent Volumes
- Git
- GitHub
- Jenkins

---

# Kubernetes Concepts Practiced

During this project, the following Kubernetes concepts were implemented and practiced:

- Pods
- ReplicaSets
- Deployments
- Services
- Namespace
- ClusterIP
- NodePort
- Ingress
- Persistent Volumes
- Persistent Volume Claims
- Helm
- Labels
- Selectors
- Rolling Updates
- Service Discovery
- Kubernetes Networking
- Container Runtime
- Kubernetes DNS

---

# Monitoring (Current Status)

Prometheus and Grafana were installed successfully using the kube-prometheus-stack Helm chart.

All monitoring Pods started successfully.

However, due to networking and DNS-related issues inside the Kubernetes cluster, external access to Grafana and complete monitoring integration could not be finalized during this project.

The installation remains available for future completion.

---

# Screenshots to Include

## 1. AWS Infrastructure
- EC2 Instances page
- Running Instances
- Security Groups

---

## 2. Kubernetes Cluster
- `kubectl get nodes -o wide`

---

## 3. Namespace
- `kubectl get namespaces`

---

## 4. Pods
- `kubectl get pods -n retail-store`

---

## 5. Deployments
- `kubectl get deployments -n retail-store`

---

## 6. Services
- `kubectl get svc -n retail-store`

---

## 7. ReplicaSets
- `kubectl get rs -n retail-store`

---

## 8. Ingress
- `kubectl get ingress -n retail-store`

---

## 9. Persistent Volume
- `kubectl get pv`

---

## 10. Persistent Volume Claim
- `kubectl get pvc -n retail-store`

---

## 11. Browser
- Retail Store Home Page

---

## 12. Helm
- `helm list -A`

---

## 13. Monitoring
- `kubectl get pods -n monitoring`

---

## 14. Project Folder Structure
- Terminal showing Kubernetes manifests and Dockerfiles

---

## 15. Jenkins Dashboard (if available)
- Jenkins homepage
- Build history
- Docker image build

---

# Suggested Repository Structure

```
retail-store-kubernetes/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── kubernetes/
│   ├── namespace.yaml
│   ├── catalog-deployment.yaml
│   ├── checkout-deployment.yaml
│   ├── orders-deployment.yaml
│   ├── ui-deployment.yaml
│   ├── carts-db.yaml
│   ├── ingress.yaml
│   ├── pv.yaml
│   └── pvc.yaml
│
├── docker/
│   ├── ui.Dockerfile
│   ├── catalog.Dockerfile
│   ├── checkout.Dockerfile
│   └── orders.Dockerfile
│
├── screenshots/
│   ├── aws/
│   ├── kubernetes/
│   ├── ingress/
│   ├── storage/
│   ├── monitoring/
│   └── application/
│
└── docs/
    └── architecture.png
```

---

# Learning Outcomes

Through this project, I gained practical experience in building and managing a production-style Kubernetes environment on AWS. I learned how to deploy and manage containerized microservices, configure Kubernetes resources, expose applications using Services and Ingress, implement persistent storage with PV/PVC, and troubleshoot real-world infrastructure issues. I also worked with Helm for package management and explored monitoring tools such as Prometheus and Grafana. This project strengthened my understanding of cloud infrastructure, container orchestration, networking, storage, and DevOps best practices while preparing me for real-world Cloud and DevOps Engineer roles.

---