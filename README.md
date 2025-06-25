# AWS-Flask-Docker-CICD 🚀🐳

A simple Python Flask web application deployed via Docker on an AWS EC2 instance using a full CI/CD pipeline powered by GitHub Actions.

---

## 📦 Tech Stack

- **Python 3 + Flask** – Lightweight web framework
- **Docker** – Containerization
- **GitHub Actions** – CI/CD automation
- **AWS EC2** – Cloud deployment target (Ubuntu-based)
- **Docker Hub** – Docker image hosting

---

## 🌐 App Overview

This Flask app deplyed on AWS displays a simple **"Hello, World!"** message and demonstrates how to:

- Dockerize a Python web app
- Automate build and push with GitHub Actions
- Deploy the Docker container to an AWS EC2 instance

---

## 🛠️ Project Structure

```bash
aws-flask-docker-cicd/
│
├── app.py           
├── requirements.txt     
├── Dockerfile           
├── .dockerignore        
├── .github/
│   └── workflows/
│       └── ci.yml       
├── README.md    

```
## 🐍 Flask App Code (app.py)

```bash 
from flask import Flask 
app = Flask(__name__)

@app.route('/')
def home():
    return "Hellooo from dockerized Flask app!!"

if __name__=='__main__':
    app.run(host='0.0.0.0', port=5000)
```
## 📜Requirements
```bash
Flask==2.3.2
```
## 🐳 Dockerfile

```bash 
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt 
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```
### Test locally:

```bash
docker build -t flask-app .
docker run -d -p 5000:5000 flask-app
```

## ☁️ Push Project to GitHub
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/aws-flask-docker-cicd.git
git push -u origin main
```

## 🤖 Set Up GitHub Actions (CI)
### Create folder: `.github/workflows/ci.yml`

```bash
# This workflow will install Python dependencies, run tests and lint with a single version of Python

name: Docker CI - Build and Push

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install dependencies 
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Log in to Docker Hub
        uses: docker/login-action@v2   
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Build Docker image  
        run: docker build -t username/flaskapp .
      
      - name: Push Docker image 
        run: docker push username/flaskapp


```
![CI Workflow](https://github.com/AmanSharma39/aws-flask-docker-cicd/blob/main/Screenshots/Screenshot%202025-06-24%20125956.png?raw=true)


## 🧪 CI/CD Pipeline (GitHub Actions) 
* `.github/workflows/ci.yml` 
- Triggers on every push to main
- Installs dependencies
- Builds Docker image
- Pushes image to Docker Hub


## 🚀 Deployment Steps
### ✅ 1. Set up AWS EC2
```bash 
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker $USER
newgrp docker
``` 
### Open the required ports:
- HTTPS
- SSH
- HTTP
- 5000 
![Docker Run on EC2](https://github.com/AmanSharma39/aws-flask-docker-cicd/blob/main/Screenshots/Screenshot%202025-06-24%20132906.png?raw=true)

### ✅ 2. GitHub Actions Secrets

In your repo, go to:
Settings → Secrets and variables → Actions → New repository secret

Add:
* `DOCKER_USERNAME` = your Docker Hub username  
* `DOCKER_PASSWORD` = your Docker Hub password or access token

![DockerHub Push](https://github.com/AmanSharma39/aws-flask-docker-cicd/blob/main/Screenshots/Screenshot%202025-06-24%20131007.png?raw=true)


### ✅ 3. Trigger CI/CD
- Push your code to the `main` branch
- GitHub Actions will build and push the Docker image
![EC2 Running](https://github.com/AmanSharma39/aws-flask-docker-cicd/blob/main/Screenshots/Screenshot%202025-06-24%20131922.png?raw=true)

### ✅ 4. Pull and Run on EC2
SSH into your EC2 instance:

```bash
docker pull dockerhub-username/flask-app
docker run -d -p 80:5000 dockerhub-username/flask-app
```
![Flask Output](https://github.com/AmanSharma39/aws-flask-docker-cicd/blob/main/Screenshots/Screenshot%202025-06-24%20132635.png?raw=true)

## 🌍 Live Demo
### http://ec2-public-ip:5000

![Final App Screenshot](https://github.com/AmanSharma39/aws-flask-docker-cicd/blob/main/Screenshots/Screenshot%202025-06-24%20132943.png?raw=true)
