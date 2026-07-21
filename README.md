# Apache Guacamole on Kubernetes

Deploy Apache Guacamole with MySQL on Kubernetes using clean and production-inspired Kubernetes manifests.

This project demonstrates how to deploy Apache Guacamole in Kubernetes while following common Kubernetes best practices such as Secrets, ConfigMaps, PersistentVolumeClaims, Health Probes and resource management.


# Features

Apache Guacamole 1.6.0

MySQL 8.0 backend

Kubernetes Deployments

ClusterIP Services

NodePort exposure

Persistent Volumes

Kubernetes Secrets

ConfigMap-based database initialization

Startup, Readiness and Liveness probes

Resource Requests & Limits

Init Container for recording directory preparation


# Repository Structure

.

├── 01-guacamole-secrets.yaml

├── 02-guacamole-initdb-configmap.yaml

├── 03-mysql-service.yaml

├── 04-mysql-pvc.yaml

├── 05-mysql-deployment.yaml

├── 06-guacamole-recordings-pvc.yaml

├── 07-guacd-service.yaml

├── 08-guacd-deployment.yaml

├── 09-guacamole-service.yaml

├── 10-guacamole-deployment.yaml

└── initdb.sql


# Components

MySQL 8.0

Persistent storage

Database initialization using ConfigMap

Startup/Readiness/Liveness probes

# guacd

The Guacamole proxy daemon responsible for handling RDP, SSH and VNC connections.

Features:

* Dedicated Deployment

* ClusterIP Service

* Persistent storage for recordings

* InitContainer for permissions

* Health probes

# Guacamole Web Application

The main web application that connects users to destination systems.

Features:

* MySQL Authentication

* guacd Integration

* Recording support

* Health Probes

* Resource limits

# Persistent Storage

This deployment creates two PersistentVolumeClaims:

mysql-data: This pvc is used for MySQL database.

guacamole-recordings: And this one is for Session recordings


# Deployment

Deploy everything in order:

```bash
kubectl apply -f .
```

OR

```bash
kubectl apply -f 01-guacamole-secrets.yaml
kubectl apply -f 02-guacamole-initdb-configmap.yaml
kubectl apply -f 03-mysql-service.yaml
kubectl apply -f 04-mysql-pvc.yaml
kubectl apply -f 05-mysql-deployment.yaml
kubectl apply -f 06-guacamole-recordings-pvc.yaml
kubectl apply -f 07-guacd-service.yaml
kubectl apply -f 08-guacd-deployment.yaml
kubectl apply -f 09-guacamole-service.yaml
kubectl apply -f 10-guacamole-deployment.yaml
```

Verify:
```bash
kubectl get pods
kubectl get svc
kubectl get pvc
```

# Access

The Guacamole web interface is exposed through a NodePort Service.

Example:
[http://Node-IP:30085](http://<Node-IP>:30085)


# Default Credentials

Guacamole initializes the database with the default administrator account:
Username: guacadmin
Password: guacadmin

It is strongly recommended to change the password after the first login.

# Learning Goals

This repository was created as part of my Kubernetes learning journey.

The primary objectives were:

* Understanding Kubernetes networking
* Working with Persistent Volumes
* Managing Secrets and ConfigMaps
* Deploying multi-container applications
* Health checking with Kubernetes probes
* Resource management
* Building reusable Kubernetes manifests

# Future Improvements
* Helm Chart
* Ingress + TLS
* External Secrets
* Cert-Manager
* Horizontal Pod Autoscaler
* Network Policies
* High Availability MySQL
* GitOps deployment with ArgoCD
