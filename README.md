# Corporate DevSecOps Pipeline

This repository defines a modular corporate pipeline for secure, containerized CI/CD and observability across distributed virtual infrastructure.

---

## Tech Stack

- GitHub
- Oracle VM / VirtualBox
- Tailscale
- Jenkins
- Maven
- SonarQube
- Trivy
- Nexus
- Docker
- Kubernetes
- SMTP (Gmail Notification)
- Helm
- Prometheus
- Grafana

---

## Phase 1: Virtual Machine Setup

- Provision a virtual machine using Oracle VirtualBox
- Install Ubuntu Jammy (22.04 LTS)
- Install and configure Tailscale for secure VPN mesh access

---

## Phase 2: Service Installation & Dependency Setup

- Install Git
- Install Docker and mount the Docker socket (`/var/run/docker.sock`) for Jenkins
- Install Jenkins with permissions to execute Docker and Kubernetes commands
- Deploy a local Kubernetes cluster using k3s
- Install Trivy for container vulnerability scanning
- Set up SonarQube for code quality analysis
- Install and configure Nexus as an internal artifact repository
- Install Helm for managing Kubernetes resource templates

---

## Phase 3: Jenkins Plugin Installation

Install the following plugins via Jenkins Plugin Manager:

- Eclipse Temurin Installer
- Pipeline Maven Integration
- Config File Provider
- SonarQube Scanner
- Kubernetes CLI
- Kubernetes
- Docker
- Docker Pipeline Step

---

## Phase 4: Pipeline Integration Flow

1. GitHub triggers Jenkins build via webhook
2. Jenkins checks out the repository and runs Maven build:
   - Compile and test code
   - Package the application
3. Trivy performs vulnerability scans:
   - File system scan
   - Docker image scan
4. SonarQube performs static code analysis
   - Jenkins waits for quality gate approval
5. Jenkins deploys packaged artifact to Nexus repository
6. Jenkins builds and tags Docker image
   - Pushes image to Docker Hub or private registry
7. Jenkins deploys application to k3s Kubernetes cluster using kubectl and Helm
8. Jenkins verifies deployment by checking pod and service status
9. Jenkins sends email notification via SMTP (Gmail) with pipeline summary and Trivy report

### Pipeline Stages (Jenkinsfile)

- Git Checkout  
- Maven Compile  
- Maven Test  
- Trivy File System Scan  
- SonarQube Analysis  
- Quality Gate Verification  
- Maven Package  
- Publish to Nexus  
- Docker Image Build & Tag  
- Trivy Image Scan  
- Docker Image Push  
- Kubernetes Deployment  
- Deployment Verification  
- Email Notification (Post Stage)

---

## Phase 5: Monitoring

- Prometheus scrapes metrics from Kubernetes workloads and services
- Grafana visualizes collected data through dashboards
- Alerts can be configured for performance thresholds and failures
