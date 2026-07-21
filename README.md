# Apache Guacamole on Kubernetes

Deploy Apache Guacamole with MySQL on Kubernetes using clean and production-inspired Kubernetes manifests.

This project demonstrates how to deploy Apache Guacamole in Kubernetes while following common Kubernetes best practices such as Secrets, ConfigMaps, PersistentVolumeClaims, Health Probes and resource management.

> **Note**
>
> This project uses the `local-path` StorageClass for PersistentVolumeClaims.
> Therefore, each component runs with a single replica.
> A future version of this project will include high-availability deployments with shared storage.

## Features

- Apache Guacamole 1.6.0

- MySQL 8.0 backend

- Kubernetes Deployments

- ClusterIP Services

- NodePort exposure

- Persistent Volumes

- Kubernetes Secrets

- ConfigMap-based database initialization

- Startup, Readiness and Liveness probes

- Resource Requests & Limits

- Init Container for recording directory preparation


## Repository Structure

```text
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
```

## Components

MySQL 8.0

Persistent storage

Database initialization using ConfigMap

Startup/Readiness/Liveness probes

## guacd

The Guacamole proxy daemon responsible for handling RDP, SSH and VNC connections.

Features:

* Dedicated Deployment

* ClusterIP Service

* Persistent storage for recordings

* InitContainer for permissions

* Health probes

## Guacamole Web Application

The main web application that connects users to destination systems.

Features:

* MySQL Authentication

* guacd Integration

* Recording support

* Health Probes

* Resource limits

## Persistent Storage

This deployment creates two PersistentVolumeClaims:

| PVC | Description |
|------|-------------|
| mysql-data | Stores MySQL database files |
| guacamole-recordings | Stores session recordings |


## Deployment

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

## Verify Deployment:

```bash
kubectl get pods
kubectl get svc
kubectl get pvc
```

## Access

The Guacamole web interface is exposed through a NodePort Service.

Example:

```text
http://<Node-IP>:30085
```


## Default Credentials

Guacamole initializes the database with the default administrator account:

```text
Username: guacadmin
Password: guacadmin
```

It is strongly recommended to change the password after the first login.

For production environments, credentials should be managed using external secret management solutions.

## Learning Goals

This repository was created as part of my Kubernetes learning journey.

The primary objectives were:

* Understanding Kubernetes networking
* Working with Persistent Volumes
* Managing Secrets and ConfigMaps
* Deploying multi-container applications
* Health checking with Kubernetes probes
* Resource management
* Building reusable Kubernetes manifests

## Lessons Learned

During this project I faced several Kubernetes challenges:

- Difference between initContainers and Jobs for database initialization
- Persistent storage behavior with local-path StorageClass
- File ownership issues when running containers with non-root users
- Using readiness and liveness probes correctly
- Managing application state with PVCs

## Future Improvements
- Helm Chart
- Kustomize support
- Ingress + TLS
- External Secrets
- Cert-Manager
- Horizontal Pod Autoscaler
- Network Policies
- High Availability MySQL
- GitOps deployment with ArgoCD

## Screenshots
### Login Page
<img width="465" height="451" alt="Screenshot 2026-07-21 232156" src="https://github.com/user-attachments/assets/53a4dc79-855d-45db-8f25-f098d7f32800" />
