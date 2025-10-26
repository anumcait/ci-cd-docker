# 🚀 CI/CD Pipeline with GitHub Actions & Docker

**CI/CD Pipeline with GitHub Actions & Docker (No Cloud Needed)**
Objective: Set up a full CI/CD pipeline that builds a Docker image, runs tests, and deploys locally.

Tools: GitHub Actions, Docker, Docker Hub (free), Minikube or local VM

**Mini Guide:**
- Write a Dockerfile and docker-compose.yml.
- Configure GitHub Actions to run tests, build the image, and push to Docker Hub.
- Use Minikube or a local VM to pull and run the image.
**Deliverables:**
 - GitHub repo with workflows
 - Docker image link
 - CI/CD workflow results
 - Screenshots of the deployed app

## 🧩 Project Overview
This project demonstrates a **local CI/CD pipeline** using **GitHub Actions**, **Docker**, and **Docker Hub** — without relying on any cloud services.  
It builds a Docker image, runs tests, pushes the image to Docker Hub, and then deploys it locally using **Docker Compose** (or optionally Minikube).

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|----------|
| **GitHub Actions** | Automate testing, building, and pushing Docker images |
| **Docker** | Containerize the application |
| **Docker Hub** | Host the built Docker images |
| **Docker Compose / Minikube** | Run and test the image locally |

---

## 📁 Project Structure
```
ci-cd-docker-demo/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .github/
│ └── workflows/
│ └── ci-cd.yml
└── README.md
```
---

## 💻 Application Code

A simple Python Flask web app.

**appy.py**

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from CI/CD with Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
---

## 🐳 Docker Setup

**Dockerfile**

```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```
docker-compose.yml
```
version: '3'
services:
  web:
    image: your-dockerhub-username/ci-cd-docker-demo:latest
    ports:
      - "5000:5000"
```
## ⚙️ GitHub Actions CI/CD Workflow

```bash
Path: .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: 3.11

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Run tests
        run: |
          echo "No tests yet, skipping..."

      - name: Build Docker image
        run: |
          docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/ci-cd-docker-demo:latest .

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Push Docker image
        run: |
          docker push ${{ secrets.DOCKERHUB_USERNAME }}/ci-cd-docker-demo:latest
```
## 🔐 GitHub Secrets Setup

Add the following secrets to your GitHub repo under
Settings → Secrets → Actions:

|Secret Name|Description|
|-----------|------------|
|DOCKERHUB_USERNAME|	Your Docker Hub username|
|DOCKERHUB_TOKEN|	Docker Hub access token|

---

## 🚀 Running the Pipeline

Push your code to the main branch.

Go to the Actions tab on GitHub.

Watch the CI/CD workflow run automatically:

✅ Checkout code

✅ Install dependencies

✅ Build Docker image

✅ Push image to Docker Hub

## 🧱 Deploy Locally

Option 1: Docker Compose
```bash
docker-compose pull
docker-compose up -d
```

Visit http://localhost:5000

You should see:

- - Hello from CI/CD with Docker!

Option 2: Minikube
```bash
minikube start
kubectl create deployment demo --image=your-dockerhub-username/ci-cd-docker-demo:latest
kubectl expose deployment demo --type=NodePort --port=5000
minikube service demo

```
## 📸 Screenshots

