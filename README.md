# Jenkins CI/CD Pipeline with Docker on AWS EC2
<p align="center">
  <img src="https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws" />
  <img src="https://img.shields.io/badge/Jenkins-CI%2FCD-red?logo=jenkins" />
  <img src="https://img.shields.io/badge/Docker-Containerization-blue?logo=docker" />
  <img src="https://img.shields.io/badge/GitHub-Automation-black?logo=github" />
</p>

## Project overview
A production-style **CI/CD pipeline** built using **Jenkins, Docker, GitHub, and AWS EC2**.

The pipeline automatically builds and deploys a Dockerized web application whenever changes are pushed to the GitHub repository.

## Architecture
<p align="center">
  <img src="Jenkins Architecture.png" alt="Jenkins Architecture" width="900">
</p>


## Tech stack

* AWS EC2 (Ubuntu)
* Docker
* Jenkins (Docker container)
* Git & GitHub
* Nginx
* HTML
* Jenkins Pipeline (Groovy)

## Features

* Automated CI/CD pipeline
* Pipeline as Code
* Docker-based Jenkins deployment
* Automatic Docker image build
* Automatic container deployment
* GitHub integration
* AWS EC2 hosting

## Project structure

```text
jenkins-docker-demo/
│── index.html
│── Dockerfile
│── Jenkinsfile
│── README.md
└── screenshots/
    ├── architecture.png
    ├── jenkins-dashboard.png
    ├── pipeline-config.png
    ├── build-success.png
    ├── console-output.png
    ├── docker-images.png
    ├── docker-ps.png
    └── website.png
```

## CI/CD workflow

### Stage 1: Checkout

Jenkins clones the latest source code from GitHub.

### Stage 2: Build

A Docker image is created from the application source.

```bash
docker build -t jenkins-demo .
```

### Stage 3: Deploy

The previous container is removed and the latest version is deployed automatically.

```bash
docker stop jenkins-demo-container || true
docker rm jenkins-demo-container || true
docker run -d --name jenkins-demo-container -p 80:80 jenkins-demo
```

## Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

## Jenkinsfile

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop jenkins-demo-container || true
                docker rm jenkins-demo-container || true
                docker run -d --name jenkins-demo-container -p 80:80 jenkins-demo
                '''
            }
        }
    }
}
```

## Jenkins deployment

Jenkins was deployed inside Docker on an AWS EC2 instance.

```bash
docker run -d \\
  --name jenkins \\
  -u root \\
  -p 8080:8080 \\
  -p 50000:50000 \\
  -v jenkins_home:/var/jenkins_home \\
  -v /var/run/docker.sock:/var/run/docker.sock \\
  jenkins/jenkins:lts
```

## Screenshots

### Architecture

![Architecture](screenshots/architecture.png)

### Jenkins dashboard

![Jenkins Dashboard](screenshots/jenkins-dashboard.png)

### Pipeline configuration

![Pipeline Configuration](screenshots/pipeline-config.png)

### Successful build

![Build Success](screenshots/build-success.png)

### Console output

![Console Output](screenshots/console-output.png)

### Docker images

![Docker Images](screenshots/docker-images.png)

### Running container

![Docker PS](screenshots/docker-ps.png)

### Deployed website

![Website](screenshots/website.png)

## AWS configuration

### Security group

| Port | Purpose |
| ---- | ------- |
| 22   | SSH     |
| 80   | Website |
| 8080 | Jenkins |

## How to run

Clone the repository:

```bash
git clone https://github.com/Divya-pillapalem8639/jenkins-docker-demo.git
cd jenkins-docker-demo
```

Build the image:

```bash
docker build -t jenkins-demo .
```

Run the container:

```bash
docker run -d -p 80:80 --name jenkins-demo-container jenkins-demo
```

Open:

```text
http://<EC2_PUBLIC_IP>
```

## Challenges and solutions

### Docker socket permission issue

**Problem**

```text
permission denied while trying to connect to the Docker daemon socket
```

**Solution**

* mounted `/var/run/docker.sock`
* ran Jenkins as root
* installed Docker CLI inside the Jenkins container

## DevOps concepts demonstrated

* Continuous Integration
* Continuous Deployment
* Pipeline as Code
* Docker containerization
* Jenkins automation
* GitHub SCM integration
* Docker volume persistence
* Docker socket mounting
* AWS EC2 deployment
* Infrastructure troubleshooting

## Resume-ready achievements

* Built an end-to-end CI/CD pipeline using Jenkins, Docker, GitHub, and AWS EC2.
* Automated Docker image creation and application deployment.
* Implemented Jenkins Pipeline as Code using a Jenkinsfile.
* Deployed Jenkins in Docker with host Docker integration.
* Resolved Docker daemon permission and deployment issues.

## Future improvements

* GitHub Webhooks
* Docker Hub integration
* Multi-stage Docker builds
* Nginx reverse proxy
* HTTPS with SSL
* Kubernetes deployment
* Prometheus & Grafana monitoring

## Author

**Divya Pillapalem**

GitHub: https://github.com/Divya-pillapalem8639

Built as part of my **Cloud & DevOps portfolio** demonstrating CI/CD automation, containerization, and AWS deployment.
