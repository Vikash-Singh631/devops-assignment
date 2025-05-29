# DevOps Take-Home Assignment

## Overview

This project demonstrates a production-grade CI/CD pipeline for a simple containerized web application deployed to a Kubernetes cluster. It incorporates security, observability, and secret management practices using widely adopted open-source tools.

---

## Architecture

Developer Push (GitHub)
|
Jenkins (CI/CD)
|
Docker Image Build
|
Vulnerability Scan (Trivy)
|
Push to Registry
|
Deploy via Helm to Minikube
|
Kubernetes Cluster
|
LOGS & MATRICS


## Tech Stack

| Category            | Tools Used                          |
|---------------------|--------------------------------------|
| Language            | Node.js
| Containerization    | Docker (Multi-stage Build, non-root) |
| CI/CD Pipeline      | Jenkins                      |
| Image Scanning      | Trivy                               |
| Kubernetes Platform | Minikube (local)                    |
| Deployment          | Helm                                |
| Secrets Management  | Kubernetes Secrets                  |
| Monitoring          | Prometheus                          |
| Logging             | stdout (can integrate with Loki/ELK) |

---

## How to Run Locally (End-to-End)

### Prerequisites

- Docker
- kubectl
- Minikube (or KinD)
- Helm
- Node.js / Python / Go (depending on app)
- GitHub CLI (for secrets)
- Trivy

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/devops-assignment.git
cd devops-assignment


## STEPS

# Build Docker Image
docker build -t myapp:latest .

# Run Trivy Vulnerability scan
trivy image myapp:latest

# Start Minikube
minikube start

# Deploy to kubernetes cluster using help
helm install myapp ./helm-chart

# Access the app
minikube service myapp

# Access using ingress
minikube tunnel
curl http://myapp.local

# Access Metrics
curl http://<app-url>/metrics

# Secrets Management
# All secrets (API keys, DB credentials) are stored securely using Kubernetes Secrets and referenced in the deployment as environment variables. Example:
kubectl create secret generic myapp-secret \
  --from-literal=API_KEY=myapikey \
  --from-literal=DB_PASSWORD=mypassword



## CI/CD Pipeline steps
# Trigger: push to main branch

1. Checkout code

2. Build Docker image

3. Scan image using Trivy

4. Push to Docker registry

5. Deploy using Helm to Minikube


## Expose Prometheus Metrics

# Expose /metrics endpoint in your app and scrape it using Prometheus.
1. Add Prometheus Client to Your App
    npm install prom-client
2. Add Annotations for Prometheus Auto-Discovery
    In your deployment.yaml (inside the pod metadata.annotations)
3.  Run Prometheus (on Minikube or external)
     Run Prometheus (on Minikube or external)
----Prometheus will now auto-discover your app and scrape metrics from /metrics---


## Limitations

1. Minikube used for simplicity; not suitable for production.
2. No RBAC policies or network policies added.
3. Trivy results are not enforced to block deployment (could be added with OPA).


## ARea of Improvement

1. Provision EKS/GKE with Terraform for real cloud deployment.
2. Set up ArgoCD for CD
3. Add OPA/Kyverno policies for image scanning/blocking.
