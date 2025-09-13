# 🎬 Book My Show - DevOps CI/CD Project

This project is a *Book My Show clone application* used for learning and demonstrating *DevOps practices* including:

## 🚀 Features
- Basic movie ticket booking functionality
- Frontend built with Node.js/React
- Backend APIs for managing movies, shows, and bookings
- Runs on Docker container

## 🔧 DevOps Implementation
This repository is used to implement a *complete CI/CD pipeline*:
1. *Source Code Management* - GitHub workflow (branching, pull requests, reviews)
2. *Build & Analysis* - Jenkins pipeline with SonarQube quality gate
3. *Security Scans* - Trivy FS scan and OWASP dependency check
4. *Containerization* - Dockerfile & DockerHub image push
5. *Deployment*  
   - Local Docker container (port 3000)  
   - Kubernetes deployment on AWS EKS / Minikube
6. *Monitoring & Observability*  
   - Prometheus + Node Exporter  
   - Grafana dashboards for system health and Jenkins metrics

## 📂 Deliverables
- Jenkinsfile (Pipeline as Code)
- Dockerfile
- Kubernetes manifests (deployment.yaml, service.yaml)
- Jenkins stage view screenshot
- Browser access proof (Docker/EKS)
- SonarQube Quality Gate report
- Prometheus & Grafana screenshots

---

✅ This repository is part of a *hands-on DevOps project* to demonstrate real-world CI/CD workflows, containerization, Kubernetes deployment, and monitoring.