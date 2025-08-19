# 🐳 Docker on AWS EC2 – Build & Push to Amazon ECR  

This repository contains step-by-step instructions and demo files for running Docker on an **Ubuntu EC2 instance**, building a Docker image, and pushing it to **Amazon Elastic Container Registry (ECR)**.  

---

## 🔍 Key Highlights  
- **EC2 Setup**: Launched an Ubuntu 22.04 instance with IAM role & security groups.  
- **Docker Installation**: Installed and configured Docker with systemd and user permissions.  
- **Sample App**: Created a simple Python application (`app.py`) and Dockerfile.  
- **Image Build**: Built Docker image (`demo-app`) directly on EC2.  
- **ECR Integration**:  
  - Created an ECR repository.  
  - Configured AWS CLI.  
  - Tagged and pushed the Docker image to ECR.  
- **Container Run**: Verified deployment by running the container on EC2 → output:  

```bash
Hello from Docker on Ubuntu EC2!
```  

---

## 🛠 Tools & Technologies  
- **AWS EC2** (Ubuntu 22.04 LTS)  
- **Docker** (image build & containerization)  
- **Amazon ECR** (image registry)  
- **AWS CLI**  

---

## 🚀 Steps Overview  

### 1️⃣ Launch Ubuntu EC2 Instance  
```bash
# Go to AWS Console → EC2 → Launch Instance  
# Choose Ubuntu 22.04 LTS → t2.micro → Attach IAM role with AmazonEC2ContainerRegistryFullAccess  
```  

### 2️⃣ Install Docker on EC2  
```bash
sudo apt update -y  
sudo apt install -y docker.io  
sudo systemctl start docker  
sudo systemctl enable docker  
sudo usermod -aG docker ubuntu  
newgrp docker  
docker --version  
```  

### 3️⃣ Create Sample Python App & Dockerfile  
```bash
mkdir demo-app && cd demo-app  
echo 'print("Hello from Docker on Ubuntu EC2!")' > app.py  

nano Dockerfile  
```  

**Dockerfile content:**  
```dockerfile
FROM python:3.9-slim  
COPY app.py /app.py  
CMD ["python", "/app.py"]  
```  

### 4️⃣ Build Docker Image  
```bash
docker build -t demo-app .  
```  

### 5️⃣ Configure AWS CLI & Create ECR Repository  
```bash
aws configure  
aws ecr create-repository --repository-name demo-app  
```  

### 6️⃣ Authenticate Docker to ECR  
```bash
aws ecr get-login-password | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com  
```  

### 7️⃣ Tag Docker Image  
```bash
docker tag demo-app:latest <account-id>.dkr.ecr.<region>.amazonaws.com/demo-app:latest  
```  

### 8️⃣ Push Image to ECR  
```bash
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/demo-app:latest  
```  

### 9️⃣ Run Container on EC2  
```bash
docker run demo-app  
```  

**Output:**  
```bash
Hello from Docker on Ubuntu EC2!
```  

---

## 📌 Purpose  
This project demonstrates a **complete DevOps workflow** of containerizing an app on EC2 and pushing images to a private container registry for scalable deployments.  
