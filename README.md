# Data Analytics Application

This project is a Python-based data analytics application designed to perform data analysis on a sample dataset. The application is containerized using Docker and deployed using Kubernetes with Minikube. A CI/CD pipeline is set up using Jenkins to automate the build, test, and deployment processes. Additionally, configuration management is handled by Ansible, and monitoring is done using AWS CloudWatch.

## Table of Contents

- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
  - [Prerequisites](#prerequisites)
  - [Local Development](#local-development)
  - [Docker](#docker)
  - [CI/CD Pipeline with Jenkins](#cicd-pipeline-with-jenkins)
  - [Deployment with Minikube](#deployment-with-minikube)
  - [Configuration Management with Ansible](#configuration-management-with-ansible)
- [Challenges Faced and Key Learnings](#challenges-faced-and-key-learnings)
- [Conclusion](#conclusion)

## Technologies Used

- **Version Control:** GitHub
- **Containerization:** Docker
- **Orchestration:** Minikube (Kubernetes)
- **Operating System:** Ubuntu
- **CI/CD Tool:** Jenkins
- **Build Tool:** Maven (optional, for Jenkins integration)
- **Testing Tools:** Pytest and Selenium
- **Configuration Management:** Ansible
- **Monitoring:** AWS CloudWatch

## Project Structure

```plaintext
├── data
│   └── sample.csv
├── src
│   ├── app.py
│   ├── analysis.py
│   └── utils.py
├── tests
│   └── test_analysis.py
├── k8s
│   ├── deployment.yaml
│   └── service.yaml
├── ansible
│   └── deploy.yaml
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
└── README.md
```

# Setup Instructions

## Prerequisites
- Python 3.8+
- Docker
- Minikube
- Jenkins
- Ansible
- AWS CloudWatch (for monitoring)

## Local Development

### Clone the Repository:

```bash
git clone https://github.com/your-username/data-analytics-app.git
cd data-analytics-app
```

# Setup Instructions

## Set Up Virtual Environment:
```bash
python3 -m venv venv
source venv/bin/activate
```

# Install Dependencies:
```bash
pip install -r requirements.txt

python3 -m venv venv
source venv/bin/activate
```

# Run the Application:
```bash
python src/app.py
```

# Docker
## Build Docker Image:
```bash
docker build -t data-analytics-app .
```

## Run Docker Container:
```bash
docker run -p 5000:5000 data-analytics-app
```

# CI/CD Pipeline with Jenkins

# Set Up Jenkins:
- Install Jenkins on an Ubuntu server or local machine.
- Install necessary Jenkins plugins (e.g., Docker, GitHub).

## Create a Jenkinsfile:
```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'pip install -r requirements.txt'
                }
            }
        }
        stage('Test') {
            steps {
                // Run Pytest tests here
                sh 'pytest'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build('data-analytics-app:latest')
                }
            }
        }
        stage('Deploy to Minikube') {
            steps {
                script {
                    // Deploy application using kubectl
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }
    }
}
```

## Configure Jenkins Job
- Create a new pipeline job in Jenkins.
- Point to the GitHub repository containing the Jenkinsfile.

# Deployment with Minikube

## Set Up Minikube
- Install Minikube and start a local cluster:
```bash
minikube start
```
# Create Kubernetes Manifests
Write deployment and service YAML files in a `k8s` directory.

## Example `deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: data-analytics-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: data-analytics-app
  template:
    metadata:
      labels:
        app: data-analytics-app
    spec:
      containers:
      - name: data-analytics-app
        image: data-analytics-app:latest
        ports:
        - containerPort: 5000
```
## Example `service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: data-analytics-service
spec:
  selector:
    app: data-analytics-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  type: NodePort
```

## Deploy to Minikube
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

## Configuration Management with Ansible

### Write Ansible Playbook

#### Example `deploy.yaml`
```yaml
---
- hosts: all
  tasks:
    - name: Install Docker
      apt:
        name: docker.io
        state: present

    - name: Deploy Application
      command: kubectl apply -f /path/to/k8s/deployment.yaml
```

## Run Ansible Playbook

```bash
ansible-playbook -i inventory ansible/deploy.yaml
```

## Challenges Faced and Key Learnings

### Challenges
- Encountered issues with Docker networking.
- Configured Jenkins plugins.
- Ensured Kubernetes cluster stability.

### Key Learnings
- Gained a deeper understanding of CI/CD pipelines.
- Learned about Kubernetes deployment strategies.
- Recognized the importance of comprehensive monitoring.

## Conclusion
This project demonstrated the integration of various tools and technologies to create a robust data analytics application with an automated CI/CD pipeline. The experience provided valuable insights into the development, deployment, and monitoring processes in a real-world scenario.

