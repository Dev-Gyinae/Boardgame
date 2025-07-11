# Corporate DevSecOps Pipeline [BoardGame Project]

This repository defines a modular corporate pipeline for secure, containerized CI/CD and observability across distributed virtual infrastructure.
<img width="790" height="482" alt="image" src="https://github.com/user-attachments/assets/818ed026-7502-4a6a-bbae-79dbe8c90a84" />


Summary:
https://github.com/Dev-Gyinae/Boardgame/blob/main/Project.md
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
Refer: https://medium.com/@dev.gyinae/building-a-home-lab-with-virtualbox-setting-up-vms-and-configuring-bridged-networking-for-remote-0e80446254e0

- Provision a virtual machine using Oracle VirtualBox
- Install Ubuntu Jammy (22.04 LTS)
- Install and configure Tailscale for secure VPN mesh access

---

## Phase 2: Service Installation & Dependency Setup

- Install Git (https://medium.com/@dev.gyinae/devops-bootcamp-version-control-with-git-d56235bd51db)
- Install Docker (https://medium.com/@dev.gyinae/devops-bootcamp-containers-with-docker-9f54469d236f) and mount the Docker socket (`/var/run/docker.sock`) for Jenkins 
- Install Jenkins with permissions to execute Docker and Kubernetes commands (https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/Jenkins.md)
- Deploy a local Kubernetes cluster using k3s (https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/kubernetes.md)
- Install Trivy for container vulnerability scanning  (https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/trivy.md)
- Set up SonarQube for code quality analysis (https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/Sonarqube.md)
- Install and configure Nexus as an internal artifact repository (https://medium.com/@dev.gyinae/how-to-install-nexus-repository-manager-on-linux-44c38a827502)
- Install Helm for managing Kubernetes resource templates (https://github.com/Dev-Gyinae/boardgame_pipeline/blob/main/helm.md)

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
<img width="1299" height="112" alt="Screenshot 2025-07-10 134742" src="https://github.com/user-attachments/assets/7dc02864-b3d1-4bd8-bd79-2c49bed67b00" />
<img width="887" height="100" alt="Screenshot 2025-07-10 134802" src="https://github.com/user-attachments/assets/60327e37-11a0-4930-a8c6-ba009b688e64" />



1. GitHub triggers Jenkins build via webhook
2. <img width="989" height="590" alt="Screenshot 2025-07-10 140921" src="https://github.com/user-attachments/assets/ff250dea-7b73-4cca-afb0-1940131f91c7" />
<img width="978" height="331" alt="Screenshot 2025-07-10 140932" src="https://github.com/user-attachments/assets/8c1e098d-464b-4988-b28b-325aa79b1f31" />

3. Jenkins checks out the repository and runs Maven build:
   - Compile and test code
   - <img width="985" height="311" alt="Screenshot 2025-07-10 140838" src="https://github.com/user-attachments/assets/8d600119-7642-4a72-a432-bdd1f85fb667" />
<img width="977" height="316" alt="Screenshot 2025-07-10 140857" src="https://github.com/user-attachments/assets/acd082a0-c21d-4aa6-b225-81e3befaa307" />

   - Package the application
   - <img width="975" height="330" alt="Screenshot 2025-07-10 140541" src="https://github.com/user-attachments/assets/baf5182e-8987-4c2b-aa22-f31f55e8daa1" />
   <img width="974" height="508" alt="Screenshot 2025-07-10 140706" src="https://github.com/user-attachments/assets/3e033f2e-99c3-464d-bd02-799e4faa071d" />


4. Trivy performs vulnerability scans:
   - File system scan
   - <img width="972" height="320" alt="Screenshot 2025-07-10 140820" src="https://github.com/user-attachments/assets/1a1d5bd7-6803-44fc-bfe5-804f811c0925" />



5. SonarQube performs static code analysis
   - Jenkins waits for quality gate approval
   - <img width="983" height="323" alt="Screenshot 2025-07-10 140449" src="https://github.com/user-attachments/assets/fc67cb76-628b-4e12-bbae-6fee330e783c" />
   <img width="976" height="436" alt="Screenshot 2025-07-10 140510" src="https://github.com/user-attachments/assets/430d4c16-b829-4632-8aec-76da0ec60480" />
   <img width="1355" height="717" alt="Screenshot 2025-07-10 142833" src="https://github.com/user-attachments/assets/dee8f71a-0a19-4b6e-b241-ba16abfa2c27" />
   <img width="1358" height="720" alt="Screenshot 2025-07-10 143042" src="https://github.com/user-attachments/assets/3a6889f2-340e-4aa4-8e52-7490b0b647c9" />
   <img width="1364" height="715" alt="Screenshot 2025-07-10 143100" src="https://github.com/user-attachments/assets/12fb695f-ae00-40ae-9c99-319567b9378f" />



6. Jenkins deploys packaged artifact to Nexus repository
7. <img width="985" height="584" alt="Screenshot 2025-07-10 141306" src="https://github.com/user-attachments/assets/e1016a3c-2bc5-4d15-9189-2d57c3c83cfe" />
<img width="1597" height="848" alt="Screenshot 2025-07-10 195317" src="https://github.com/user-attachments/assets/8b4dcbde-1be7-4998-b348-e641b141f677" />
<img width="1586" height="564" alt="Screenshot 2025-07-10 195340" src="https://github.com/user-attachments/assets/e1a48418-367c-410e-ad1f-9ef78cc65903" />
<img width="1591" height="465" alt="Screenshot 2025-07-10 195405" src="https://github.com/user-attachments/assets/c80480ae-282d-45d7-814a-76e4ad6d99cb" />


8. Jenkins builds and tags Docker image
   - Docker image scan
   - <img width="981" height="326" alt="Screenshot 2025-07-10 141430" src="https://github.com/user-attachments/assets/ef7956da-f8b3-402d-956f-ff35e79c6079" />

   - Pushes image to Docker Hub or private registry
   - <img width="992" height="593" alt="Screenshot 2025-07-10 141451" src="https://github.com/user-attachments/assets/2da29250-c40c-4c9b-b61d-a1fd7628ba57" />

10. Jenkins deploys application to k3s Kubernetes cluster using kubectl and Helm
11. Jenkins verifies deployment by checking pod and service status
    <img width="741" height="243" alt="Screenshot 2025-07-10 132729" src="https://github.com/user-attachments/assets/79c73eb1-5f22-4293-863b-90d6cbffdf94" />
    

12. Jenkins sends email notification via SMTP (Gmail) with pipeline summary and Trivy report
<img width="1272" height="260" alt="Screenshot 2025-07-10 134930" src="https://github.com/user-attachments/assets/44b537b5-7e42-426f-aa38-bc3ed05efcc5" />
<img width="1354" height="683" alt="Screenshot 2025-07-10 135445" src="https://github.com/user-attachments/assets/b9ec598a-3e0d-492a-86dd-2d50deebf86f" />
<img width="1354" height="682" alt="Screenshot 2025-07-10 135753" src="https://github.com/user-attachments/assets/2b766781-cc88-4a6e-8d8d-64355cf69a2c" />
<img width="1360" height="681" alt="Screenshot 2025-07-10 140023" src="https://github.com/user-attachments/assets/231f4dcd-4f91-4b1a-a643-b2eeb0905c46" />
<img width="991" height="186" alt="Screenshot 2025-07-10 141510" src="https://github.com/user-attachments/assets/bc96f231-9713-4220-bdee-7df114b9d0cd" />
<img width="1049" height="564" alt="Screenshot 2025-07-10 141656" src="https://github.com/user-attachments/assets/ce18dd49-6c61-45e0-969c-e47f42cd7925" />
<img width="1339" height="639" alt="Screenshot 2025-07-10 141926" src="https://github.com/user-attachments/assets/d7bba76f-ee34-4f49-9954-430daffc58a0" />
<img width="1004" height="435" alt="Screenshot 2025-07-10 142010" src="https://github.com/user-attachments/assets/0b63bbcb-d0c1-455f-b10f-764667ba1cce" />


### Pipeline Stages (Jenkinsfile)

- Git Checkout
  <img width="978" height="331" alt="Screenshot 2025-07-10 140932" src="https://github.com/user-attachments/assets/1beff7a7-a672-443f-8659-3aed38d8f700" />
  <img width="989" height="590" alt="Screenshot 2025-07-10 140921" src="https://github.com/user-attachments/assets/ac71e174-2bfd-4378-bd52-340e9e70c34d" />


- Maven Compile
<img width="977" height="316" alt="Screenshot 2025-07-10 140857" src="https://github.com/user-attachments/assets/08d094e0-cc32-4a43-a643-dbdaecf363a0" />

- Maven Test
<img width="985" height="311" alt="Screenshot 2025-07-10 140838" src="https://github.com/user-attachments/assets/d305eebf-e087-4579-9844-085a4705bd85" />


- Trivy File System Scan
<img width="972" height="320" alt="Screenshot 2025-07-10 140820" src="https://github.com/user-attachments/assets/51fda2ad-2dfd-44f4-81af-c7dc87cb13fd" />
 
- SonarQube Analysis
  <img width="983" height="323" alt="Screenshot 2025-07-10 140449" src="https://github.com/user-attachments/assets/42545da2-2f45-44b3-87b5-82f9952ce4d4" />
  
- Quality Gate Verification
- <img width="976" height="436" alt="Screenshot 2025-07-10 140510" src="https://github.com/user-attachments/assets/ddffdf7a-2958-47db-afd0-f43fa69bf366" />

- Maven Package
  <img width="975" height="330" alt="Screenshot 2025-07-10 140541" src="https://github.com/user-attachments/assets/14939896-1975-470a-809a-be52158534a8" />
  <img width="974" height="508" alt="Screenshot 2025-07-10 140706" src="https://github.com/user-attachments/assets/6d9fb262-3efb-4453-b094-06fd58ebb254" />
  


- Publish to Nexus
  <img width="985" height="584" alt="Screenshot 2025-07-10 141306" src="https://github.com/user-attachments/assets/9eaa348f-66b8-46d2-98ca-36d97badac0f" />
  

- Docker Image Build & Tag  
- Trivy Image Scan
  <img width="981" height="326" alt="Screenshot 2025-07-10 141430" src="https://github.com/user-attachments/assets/ffbfb7ac-e558-46c3-b0f2-39bfb88473f4" />

- Docker Image Push
  <img width="992" height="593" alt="Screenshot 2025-07-10 141451" src="https://github.com/user-attachments/assets/36a3e1cd-1a52-4934-bc50-5db7e3642fb1" />

- Kubernetes Deployment  
- Deployment Verification  
- Email Notification (Post Stage)
- <img width="991" height="186" alt="Screenshot 2025-07-10 141510" src="https://github.com/user-attachments/assets/46530abe-838d-46b0-8c19-c3b4ba2259dd" />
<img width="1339" height="639" alt="Screenshot 2025-07-10 141926" src="https://github.com/user-attachments/assets/4e17f3f5-e350-47ef-ad5d-ef7d077a84b8" />
<img width="1004" height="435" alt="Screenshot 2025-07-10 142010" src="https://github.com/user-attachments/assets/b545f419-6561-48fd-8bd0-89b875c5a3a3" />




---

## Phase 5: Monitoring
Refer:  https://github.com/Dev-Gyinae/monitoring_stack

- Prometheus scrapes metrics from Kubernetes workloads and services
- Grafana visualizes collected data through dashboards
- Alerts can be configured for performance thresholds and failures



## Refer to this repo for executed commands: https://github.com/Dev-Gyinae/boardgame_pipeline/tree/main 
