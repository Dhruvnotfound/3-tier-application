# 3-Tier Application Deployment

This project demonstrates how to deploy a 3-tier application (React frontend, Node.js backend, and MongoDB database) to a Kubernetes cluster using Docker images and Helm.

## Prerequisites

- Docker installed on your local machine
- Kubernetes cluster (local or cloud-based)
- Helm package manager installed on your local machine

## Application Overview

The 3-tier application consists of the following components:

1. **Frontend**: A React application serving the user interface.
2. **Backend**: A Node.js application that handles the business logic and interacts with the database.
3. **MongoDB**: A NoSQL database for storing application data.

## Getting Started

### 1. Build Docker Images

- Navigate to the `frontend` directory and build the Docker image for the React application:
  ```bash
  cd frontend
  docker build -t your-docker-username/frontend:1.0.0 .
  ```
- Navigate to the `backend` directory and build the Docker image for the Node.js application:
  ```bash
  cd ../backend
  docker build -t your-docker-username/backend:1.0.0 .
  ```
- Push the Docker images to a container registry (e.g., Docker Hub, ECR, GCR):
  ```bash
  docker push your-docker-username/frontend:1.0.0
  docker push your-docker-username/backend:1.0.0
  ```

### 2. Deploy with Helm

- Navigate to the `3-tier-app` directory:
  ```bash
  cd 3-tier-app
  ```
- Update the `values.yaml` file with the appropriate Docker image tags and any other configuration settings.
- Install the Helm chart:
  ```bash
  helm install 3-tier-app .
  ```
- This will deploy the 3-tier application to your Kubernetes cluster, including the following resources:
  - Namespace (3-tier)
  - MongoDB StatefulSet
  - Secrets for Mongo Auth
  - PersistentVolume to attach to Mongo database
  - Backend Deployment
  - Frontend Deployment
  - MongoDB Service
  - Backend Service
  - Frontend Service
  - Ingress (optional)

### 3. Access the Application

- After the deployment is complete, you can access the frontend application using the provided ingress or load balancer URL.
- Alternatively, you can use `kubectl port-forward` to access the frontend service locally:
  ```bash
  kubectl port-forward service/frontend 8080:80
  ```
  Then, open `http://localhost:8080` in your web browser.

## Customization

You can customize the deployment by modifying the following files:

- `helm/values.yaml`: Update environment variables, resource requests/limits, and other configurations.
- `helm/templates/`: Modify the Kubernetes manifests for each component (Deployments, Services, Ingress, etc.).

## Cleanup

To remove the 3-tier application from your Kubernetes cluster, run the following command:

```bash
helm uninstall 3-tier-app
```

This will delete all the resources associated with the application.

### Minikube

If you're using Minikube, follow these additional steps:

1. **Start Minikube**:
   ```bash
   minikube start
   ```

2. **Build Docker Images**: Skip this step if you've already built and pushed the Docker images.

3. **Deploy with Helm**: Follow the instructions in the "Deploy with Helm" section above.

4. **Access the Application**: Follow the instructions in the "Access the Application" section above.

After cleaning up, you can stop the Minikube cluster by running:

```bash
minikube stop
```
