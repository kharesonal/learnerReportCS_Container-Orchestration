# MERN Stack Deployment with Kubernetes, Helm, and Jenkins

## Description:

This repository outlines the deployment of a MERN (MongoDB, Express.js, React.js, Node.js) application using Kubernetes for container orchestration, Helm for deployment automation, and Jenkins for CI/CD pipeline integration.

**The project involves two components:**

- Frontend (React.js)
- Backend (Node.js + Express.js)

The project uses MongoDB as the database. The application will be deployed in a Kubernetes cluster, with Helm charts managing deployment and Jenkins automating the build and deployment process.

## Prerequisites:

Make sure the following tools are installed and configured on your local machine or server:

1.Docker

2.Kubernetes (Minikube)

3.Helm

4.Jenkins

5.kubectl

6.Git

## Execution Steps:

**Step 1: Clone the Frontend and Backend Repositories**
```
git clone https://github.com/UnpredictablePrashant/learnerReportCS_frontend.git
git clone https://github.com/UnpredictablePrashant/learnerReportCS_backend.git
```
**Step 2: Build Docker Images for Frontend and Backend**

Navigate into each directory and build the Docker images for both frontend and backend:

**Frontend:**
```
cd learnerReportCS_frontend
docker build -t ${DOCKER_HUB_REPO}:frontend-${VERSION} .
docker push ${DOCKER_HUB_REPO}:frontend-${VERSION}
```

**Backend:** 

```
cd learnerReportCS_backend
docker build -t ${DOCKER_HUB_REPO}:backend-${VERSION} .
docker push ${DOCKER_HUB_REPO}:backend-${VERSION}
```

Replace ${DOCKER_HUB_REPO} with the docker hub repository tag (e.g., sonalsrivastava1189/learner_report).

Replace ${VERSION} with the desired version tag (e.g., v1.0.0).

**Step3:Kubernetes Deployment**

Kubernetes manifest files for each component (backend, frontend, and database) are available in the Backend-deployment,Frontend-deployment,Database directory.

These YAML files define the deployment, service, and namespace configurations.

You can manually apply these manifests using kubectl:

```
kubectl apply -f backend-deployment/
kubectl apply -f frontend-deployment/
kubectl apply -f database/
```

**Step 4: Helm Chart**

The learnerReportHelm directory contains Helm charts to streamline the deployment.

To install or upgrade the Helm release, use:

```
helm upgrade --install learnerreport ./learnerReport_helm --set backend.image=${DOCKER_HUB_REPO}:backend-${VERSION} --set frontend.image=${DOCKER_HUB_REPO}:frontend-${VERSION}
```

This will deploy all components (backend, frontend, and database) into your Kubernetes cluster.

**Step5: Jenkins Pipeline**

Create a Jenkinsfile to automate the deployment using Jenkins.

This pipeline checks out the frontend and backend code, builds Docker images, pushes them to Docker Hub, and then deploys them to your Kubernetes cluster using Helm.

**Jenkins Authentication setup:**

**Step 1: Configure Docker Hub Credentials in Jenkins**

To push Docker images to your Docker Hub or private registry, you need to configure credentials in Jenkins.

 1.Go to Jenkins dashboard -> Manage Jenkins -> Manage Credentials.
 
 2. Add new credentials:
   - Username: Your Docker Hub or registry username.
   - Password: Your Docker Hub or registry password.
   - ID: dockerhub-credentials (used in the Jenkinsfile).

**Step 2: Configure Kubernetes Credentials in Jenkins**

1.Save your Kubeconfig file (usually located at ~/.kube/config) locally.

```
kubectl config view --raw
```
Copy and save it's output in Kubeconfig file.

2. Go to Jenkins dashboard -> Manage Jenkins -> Manage Credentials.
   
3.Add new credentials:

  - Kind: Secret file
  - File: Upload your Kubeconfig file.
  - ID: kubeconfig-credentials.

This Kubeconfig file will be used by Jenkins to access the Kubernetes cluster during the deployment process.

**Step 6: Monitoring and Testing**

You can monitor the deployment in Kubernetes using:

```
kubectl get all --namespace=backendlearner
kubectl get all --namespace=learnerreport-database
kubectl get all --namespace=frontendlearner
```
