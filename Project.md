# Corporate DevSecOps Pipeline – BoardGame Project

A modular, secure CI/CD pipeline for building, analyzing, containerizing, and deploying board game applications in a Kubernetes environment, complete with monitoring and alerting.

\[pipeline-overview image]

## Tech Stack

* Source Control: GitHub
* Compute & Access: Oracle VirtualBox, Tailscale VPN
* CI/CD: Jenkins, Maven
* Security: Trivy (FS & image scans), SonarQube (code analysis)
* Artifact Management: Nexus
* Containerization: Docker
* Orchestration: Kubernetes (k3s), Helm
* Monitoring: Prometheus, Grafana
* Notifications: Gmail SMTP

## Phase 1: Infrastructure Setup

Objective: Provision a secure, virtualized lab environment.

1. Provision a VM with Oracle VirtualBox
2. Install Ubuntu 22.04 LTS (Jammy)
3. Set up Tailscale for encrypted remote access
   Guide: [https://medium.com/@dev.gyinae/building-a-home-lab-with-virtualbox-setting-up-vms-and-configuring-bridged-networking-for-remote-0e80446254e0](https://medium.com/@dev.gyinae/building-a-home-lab-with-virtualbox-setting-up-vms-and-configuring-bridged-networking-for-remote-0e80446254e0)

## Phase 2: Service & Dependency Installation

Objective: Set up the CI/CD tooling stack.

* Install Git, Docker, and Jenkins with required permissions
* Install and configure:

  * Kubernetes (k3s) – https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/kubernetes.md
  * Trivy – https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/trivy.md
  * SonarQube – https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/Sonarqube.md
  * Nexus Repository – [https://medium.com/@dev.gyinae/how-to-install-nexus-repository-manager-on-linux-44c38a827502](https://medium.com/@dev.gyinae/how-to-install-nexus-repository-manager-on-linux-44c38a827502)
  * Helm – https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/helm.md

## Phase 3: Jenkins Plugin Configuration

Install these plugins via the Jenkins Plugin Manager:

* Eclipse Temurin Installer
* Pipeline: Maven Integration
* Config File Provider
* SonarQube Scanner
* Docker & Docker Pipeline
* Kubernetes & Kubernetes CLI

## Phase 4: CI/CD Pipeline Flow

High-level Workflow:

1. Trigger: GitHub webhook triggers Jenkins
2. Code Checkout: Jenkins pulls source code
3. Build & Test: Maven compiles and tests
4. Security Scan: Trivy scans the filesystem
5. Code Quality: SonarQube analyzes and blocks on quality gate
6. Packaging: Maven packages and uploads to Nexus
7. Docker Image: Jenkins builds, tags, and scans image
8. Push Image: Docker image is pushed to DockerHub/private registry
9. Deploy: Application deployed to k3s via kubectl and Helm
10. Verify: Deployment and pod health checked
11. Notify: Email sent via SMTP (Trivy results + summary)

Each stage includes visual logs and screenshots for reference.

## Phase 5: Jenkinsfile Pipeline Stages

| Stage               | Description                         |
| ------------------- | ----------------------------------- |
| checkout            | Clone code from GitHub              |
| maven\_compile      | Compile Java project                |
| maven\_test         | Run unit/integration tests          |
| trivy\_fs\_scan     | Filesystem-level vulnerability scan |
| sonarqube\_analysis | Static code quality check           |
| quality\_gate       | Pause until SonarQube approval      |
| maven\_package      | Package app as .jar or .war         |
| nexus\_publish      | Upload to Nexus Repository          |
| docker\_build       | Build and tag Docker image          |
| trivy\_image\_scan  | Security scan on Docker image       |
| docker\_push        | Push image to container registry    |
| k8s\_deploy         | Deploy to k3s using Helm            |
| verify\_deploy      | Check pods/services status          |
| notify              | Send email with summary/report      |

## Phase 6: Observability Stack

Monitoring Setup Repo: [https://github.com/Dev-Gyinae/monitoring\_stack](https://github.com/Dev-Gyinae/monitoring_stack)

* Prometheus: Scrapes Kubernetes metrics
* Grafana: Visual dashboards for services
* Alerts: Configurable for failures and thresholds

## Additional Resources

* Full Pipeline Source & Scripts: [https://github.com/Dev-Gyinae/boardgame\_pipeline/tree/main](https://github.com/Dev-Gyinae/boardgame_pipeline/tree/main)
* Executed Commands: [https://github.com/Dev-Gyinae/boardgame\_pipeline/tree/main/scripts](https://github.com/Dev-Gyinae/boardgame_pipeline/tree/main/scripts)
