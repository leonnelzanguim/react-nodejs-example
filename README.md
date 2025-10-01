# 📄 README – Deploying a Node.js/React Application on AWS EC2 with Docker

## 🚀 Steps Performed

### 1. Launch an EC2 Instance
- Created an **Amazon Linux 2** instance (e.g., t2.micro).  
- Connected via SSH:  
  ```bash
  ssh -i my-key.pem ec2-user@<EC2_PUBLIC_IP>
  ```

### 2. Install Docker
- Update the system:  
  ```bash
  sudo yum update -y
  ```
- Install Docker:  
  ```bash
  sudo yum install docker -y
  ```
- Start and enable Docker service:  
  ```bash
  sudo systemctl start docker
  sudo systemctl enable docker
  ```
- Add the `ec2-user` to the Docker group (to avoid using `sudo`):  
  ```bash
  sudo usermod -aG docker ec2-user
  ```

### 3. Configure Firewall (AWS Security Groups)
- Allowed inbound traffic for:  
  - **Port 22 (SSH)**  
  - **Port 3080 (application)**  
- Denied all other ports by default.  

### 4. Build Docker Image
- Used a **multi-stage Dockerfile**:  
  - **Stage 1**: Build the React frontend (`npm run build`).  
  - **Stage 2**: Install Node.js backend dependencies and copy the built React files.  

- Build the image:  
  ```bash
  docker build -t myapp:latest .
  ```

### 5. Push Image to Docker Hub
- Log in to Docker Hub:  
  ```bash
  docker login
  ```
- Tag the image:  
  ```bash
  docker tag myapp:latest <DOCKERHUB_USERNAME>/myapp:latest
  ```
- Push to Docker Hub:  
  ```bash
  docker push <DOCKERHUB_USERNAME>/myapp:latest
  ```

### 6. Deploy on EC2
- Pull the image from Docker Hub:  
  ```bash
  docker pull <DOCKERHUB_USERNAME>/myapp:latest
  ```
- Run the container exposing port 3080:  
  ```bash
  docker run -d -p 3080:3080 <DOCKERHUB_USERNAME>/myapp:latest
  ```

### 7. Access the Application
- Open in browser:  
  ```
  http://<EC2_PUBLIC_IP>:3080
  ```

---

## ✅ Summary
1. EC2 instance launched on AWS  
2. Docker installed and configured  
3. Firewall rules set with Security Groups  
4. Docker image built using multi-stage Dockerfile  
5. Image pushed to Docker Hub  
6. Image deployed on EC2  
7. Application running inside a container, accessible via port **3080**
