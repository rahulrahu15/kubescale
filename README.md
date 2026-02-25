# KubeScale: Production-Grade Kubernetes Project with CI/CD and Monitoring

## Project Overview
KubeScale is a production-ready Kubernetes microservices project designed to demonstrate full-stack deployment, CI/CD automation, and observability. The project includes:

- **HTML-based sample app** served via Nginx
- **Kubernetes deployments** for blue-green releases
- **CI/CD pipeline** using GitHub Actions
- **Dockerized applications** stored in Amazon ECR
- **LoadBalancer service** with `sslip.io` for easy access
- **Monitoring stack** with Grafana and Prometheus

---

## Architecture Diagram
                 +---------------------+
                 |    GitHub Repo      |
                 |  (Code + Dockerfile)|
                 +----------+----------+
                            |
                            v
                  GitHub Actions CI/CD
                  -------------------
            - Build Docker Image
            - Push to Amazon ECR
            - Update Kubernetes Manifests
            - Apply to EKS Cluster
                            |
                            v
        +---------------------------------+
        |          Amazon EKS             |
        |  Kubernetes Cluster             |
        |                                 |
        |  +---------------------------+  |
        |  | Deployment (Blue/Green)   |  |
        |  | Pods running Nginx HTML   |  |
        |  +---------------------------+  |
        |                                 |
        |  +---------------------------+  |
        |  | LoadBalancer Service      |  |
        |  | External Access           |  |
        |  +---------------------------+  |
        |                                 |
        |  +---------------------------+  |
        |  | Monitoring Stack          |  |
        |  | Prometheus + Grafana      |  |
        |  +---------------------------+  |
        +---------------------------------+
                            |
                            v
                  Public Access via
               ELB / sslip.io URL


**Description:**

1. **Docker**: Containers for the app and HTML content
2. **Amazon EKS**: Managed Kubernetes cluster
3. **CI/CD Pipeline**: GitHub Actions builds Docker images, pushes to ECR, updates Kubernetes manifests
4. **Blue-Green Deployment**: Ensures zero-downtime updates
5. **Monitoring**: Prometheus collects metrics, Grafana visualizes them
6. **LoadBalancer + sslip.io**: Provides stable public access to the service

---

## Folder Structure
kubescale/
├── .github/
│ └── workflows/
│ └── deploy.yml # CI/CD workflow
├── app/
│ └── Dockerfile # Dockerfile for HTML app
├── k8s/
│ ├── deployment.yaml # Kubernetes Deployment manifests
│ ├── service.yaml # Kubernetes Service manifests
│ └── html/
│ ├── Dockerfile
│ └── index.html
├── terraform/ # Infrastructure as Code
│ ├── eks.tf
│ ├── provider.tf
│ ├── vpc.tf
│ ├── iam_policy.json
│ ├── outputs.tf
│ └── variables.tf
└── README.md



---

## Deployment Instructions

### Prerequisites

- AWS account with EKS, IAM, and ECR permissions
- kubectl installed and configured
- Docker installed
- GitHub repository with secrets:  
  - `AWS_ACCESS_KEY_ID`  
  - `AWS_SECRET_ACCESS_KEY`  

### Steps

1. Clone the repository:
```bash
git clone https://github.com/rahulrahu15/kubescale.git
cd kubescale
2. Deploy Terraform infrastructure:
terraform init
terraform apply
3. Build and deploy the app manually (optional, CI/CD handles this automatically):
cd k8s/html
docker build -t <your-ecr-uri>:latest .
docker push <your-ecr-uri>:latest
kubectl apply -f ../deployment.yaml
kubectl apply -f ../service.yaml
4. CI/CD is triggered automatically on push to main branch via GitHub Actions.
5. Access the app:
  ELB URL: (from kubectl get svc)
  SSLip.io URL: http://<external-ip>.sslip.io example http://13.205.112.107.sslip.io



---

## CI/CD Pipeline

1. Push to `main` branch triggers GitHub Actions  
2. Action builds Docker image for the app  
3. Pushes image to AWS ECR  
4. Updates Kubernetes manifests with new image  
5. Applies manifests to EKS cluster (blue-green deployment)  

---

## Monitoring & Observability

- **Prometheus**: Collects metrics from pods  
- **Grafana**: Dashboards visualize CPU, memory, request stats  
- Access Grafana via port-forward or LoadBalancer  

---

## Accessing the App

- Public access via LoadBalancer + `sslip.io` domain  
- Example: `http://nginx.<your-public-ip>.sslip.io`  
- Zero downtime thanks to blue-green deployment

### *Outcome*

Fully automated Kubernetes deployment
Blue-green deployment with zero downtime
CI/CD ensures continuous integration and delivery
Monitoring with Prometheus and Grafana
Accessible publicly via ELB or sslip.io link

