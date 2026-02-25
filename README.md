KubeScale – Production-Grade Kubernetes CI/CD Project

KubeScale is a hands-on project demonstrating an end-to-end Kubernetes deployment with CI/CD, Terraform-managed infrastructure, Dockerized applications, and monitoring using Prometheus & Grafana.

Project Overview

KubeScale simulates a real-world microservice deployment workflow with:
Infrastructure as Code using Terraform
Amazon EKS cluster deployment
Dockerized HTML application
End-to-end CI/CD pipeline via GitHub Actions
Observability stack: Prometheus + Grafana
Blue/Green deployment support for zero-downtime updates
This project showcases cloud automation, container orchestration, and monitoring best practices.

Architecture Diagram
Terraform provisions VPC, subnets, IAM roles, and EKS cluster
Docker images are built and pushed to Amazon ECR
GitHub Actions CI/CD deploys updated images to the cluster
Services exposed via LoadBalancer for public access
Prometheus & Grafana monitor pod metrics

Project Structure
kubescale/
├─ .github/workflows/   # CI/CD GitHub Actions workflow
│   └─ deploy.yml
├─ k8s/                # Kubernetes manifests
│   ├─ deployment.yaml
│   ├─ service.yaml
│   └─ html/           # Dockerized HTML app
│       ├─ Dockerfile
│       └─ index.html
├─ terraform/          # IaC for AWS resources
│   ├─ eks.tf
│   ├─ vpc.tf
│   ├─ provider.tf
│   ├─ variables.tf
│   ├─ outputs.tf
│   └─ iam_policy.json
├─ app/                # Optional: other app-related code
└─ .gitignore

CI/CD Pipeline

Triggered on push to main branch
Build Docker image from HTML app
Push image to Amazon ECR
Update Kubernetes deployment with new image
Apply manifests to EKS cluster
Fully automated: build → push → deploy

Screenshots
Grafana Dashboard
Prometheus 

Tech Stack

Cloud: AWS EKS, VPC, ELB, IAM, ECR
Containerization: Docker, Nginx
Orchestration: Kubernetes
IaC: Terraform
CI/CD: GitHub Actions
Monitoring: Prometheus, Grafana
Version Control: Git, GitHub

Future Improvements
Add Blue/Green or Canary deployments fully automated
Integrate Secrets Management with AWS Secrets Manager
Add more microservices for a multi-tier app
Enable auto-scaling for pods based on metrics
Outcome

Fully automated Kubernetes deployment workflow
CI/CD ensures zero-downtime updates
Real-time monitoring with Grafana
Accessible via ELB URL or convenient sslip.io
Portable, production-ready infrastructure setup
