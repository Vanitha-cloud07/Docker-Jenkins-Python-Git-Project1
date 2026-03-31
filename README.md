# Jenkins-DockerHub CI/CD Project

---

## Project Overview

This project demonstrates an end-to-end CI/CD pipeline where Jenkins builds a Docker image for a Python Flask application, pushes it to Docker Hub, and deploys the application on a container on a local machine..

---

## Architecture

GitHub → Jenkins → Docker Build → Docker Hub → EC2 → Flask App  

- GitHub stores application source code  
- Jenkins pulls code and runs the CI/CD pipeline  
- Docker builds the application image  
- Docker Hub stores versioned images  
- EC2 pulls and runs the latest container  
- Flask app is deployed and verified  

---

## Tech Stack

- Jenkins – CI/CD automation  
- Docker – Containerization  
- Docker Hub – Image registry  
- AWS EC2 – Deployment server  
- Python Flask – Web application  
- GitHub – Source code management  

---

## CI/CD Pipeline

### 1. Checkout Code
- Jenkins pulls code from GitHub repository  

### 2. Build Docker Image
docker build -t labubu67/my-flask-app:${BUILD_NUMBER} .

### 3. Login to Docker Hub
echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin  

### 4. Push Docker Image
docker push labubu67/my-flask-app:${BUILD_NUMBER}  
docker tag labubu67/my-flask-app:${BUILD_NUMBER} YOUR_DOCKERHUB_USERNAME/my-flask-app:latest  
docker push labubu67/my-flask-app:latest  

### 5. Stop Old Container
docker stop flask-project2 || true  
docker rm flask-project2 || true  

### 6. Pull Latest Image
docker pull labubu67/my-flask-app:latest  

### 7. Run New Container
docker run -d -p 5000:5000 --name flask-project2 labubu67/my-flask-app:latest  

### 8. Verify Application
curl http://localhost:5000  

---

## How to Run

1. Access Jenkins  
Access Jenkins on local machine  

2. Configure Pipeline  
Connect Jenkins to GitHub repository  
Add Docker Hub credentials  

3. Run Pipeline  
Execute Jenkins pipeline  

4. Automatic Trigger (Optional)  
Configure GitHub webhook to trigger pipeline on code push  

---

## Application Output

http://localhost:5000  

Expected response:  
Hello from Project 2 - Docker Hub CI/CD pipeline!  

---

## Pipeline Result

Application successfully built and pushed to Docker Hub  
Docker image versioned using build number  
Container deployed from Docker Hub image  
Jenkins pipeline completed with SUCCESS  
Application verified after deployment  

---

## Key Features

Docker image build and versioning  
Integration with Docker Hub registry  
Automated CI/CD pipeline using Jenkins  
Deployment from registry (not local image)  
End-to-end automated workflow
