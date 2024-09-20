
A Go-based application deployed using Docker and Kubernetes, consisting of three tiers: proxy, backend, and database. The project runs in a Kubernetes cluster with services and configurations managed through YAML files.

## Table of Contents
1. [Introduction](#introduction)
2. [Technologies Used](#technologies-used)
3. [Architecture](#architecture)
4. [Prerequisites](#prerequisites)
5. [Installation](#installation)
6. [Running the Application](#running-the-application)
7. [Configuration](#configuration)
8. [License](#license)

## Introduction

This project demonstrates the deployment of a multi-tier Go application in a Kubernetes environment. It consists of three main components:

- **Proxy**: Handles external requests and forwards them to the backend service.
- **Backend**: Core logic of the application.
- **Database**: Stores persistent data.

All components are containerized and deployed in a Kubernetes cluster.

## Technologies Used

- **Go**: Backend development
- **Docker**: Containerization
- **Kubernetes**: Orchestration
- **Minikube**: Local Kubernetes development
- **DockerHub**: Image repository

## Architecture

The project is organized into three tiers:

- **Proxy Tier**: Handles traffic, communicates with the backend service.
- **Backend Tier**: Processes requests and communicates with the database.
- **Database Tier**: Stores application data.

### Key Components:

- **Deployments**: Each tier is deployed as a Kubernetes Deployment with 2 replicas.
- **Services**: Backend and database tiers are exposed using internal services.
- **Namespace**: The entire project is deployed within the `webapp` namespace.
- **Volume**: Secrets for the database credentials are mounted on the host.

## Prerequisites

- **Minikube**: Ensure Minikube is installed and running.
- **Docker**: Docker should be installed to build and push images.
- **kubectl**: Required to interact with the Kubernetes cluster.
- **DockerHub Account**: To push and pull Docker images.

## Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/joisyousef/Orange-Kubernetes-Project.git
   cd Orange-Kubernetes-Project
   ```

2. **Build Docker images**:
   Build and push the backend, proxy, and database images to DockerHub.

   Example for backend:
   
   ```bash
   docker build -t your-dockerhub-username/backend:latest ./backend
   docker push your-dockerhub-username/backend:latest
   ```

   Repeat for the proxy and database.

3. **Start Minikube**:

   ```bash
   minikube start
   ```

## Running the Application

1. **Create a namespace**:

   ```bash
   kubectl create namespace webapp
   ```

2. **Deploy the application**:
   Apply the YAML files to deploy the proxy, backend, and database:

```bash
kubectl apply -f Deployments/Backend-Deployment.yaml
kubectl apply -f Deployments/Proxy-Deployment.yaml
kubectl apply -f Deployments/Database-Deployment.yaml
kubectl apply -f Services/Backend-Service.yaml
kubectl apply -f Services/Nodeport.yaml
kubectl apply -f Services/Database-Service.yaml
kubectl apply -f Volumes/Database-pv.yaml
kubectl apply -f Volumes/Databasepvc.yaml
kubectl apply -f Volumes/Database-Secret.yaml
	```

3. **Expose the services**:
   Expose the proxy service to access the application:
   
   ```bash
   kubectl expose deployment proxy-deployment --type=NodePort --port=80 --name=proxy-service
   ```

4. **Access the application**:
   Get the Minikube IP:
   
   ```bash
   minikube ip
   ```
   
   Access the application at `http://<minikube-ip>:<node-port>`.

## Configuration

- **Database Credentials**: Ensure the secret with database credentials is mounted properly in the database deployment YAML.

The output should look like this:

![[Pasted image 20240920142915.png]]

---

**Terminate the application**

```
kubectl delete deployments -n webapp --all
kubectl delete pvc -n webapp --all
kubectl delete pv db-pv
kubectl delete services --all -n webapp
kubectl delete secrets --all -n webapp
```

