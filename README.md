# Turbo Octo Bassoon 🚀

A simple Node.js web app deployed using a full CI/CD pipeline powered by Jenkins and Docker on AWS EC2. Every GitHub push automatically triggers a build, rebuilds the Docker image, and redeploys the application.

---

## 🌐 Live Demo

🔗 http://13.203.97.18:3000/

---

## 🛠 Tech Stack

- Node.js
- Docker
- Jenkins
- AWS EC2 (Ubuntu)
- GitHub Webhooks

---

## 📦 Project Structure
```bash
.
├── .env
├── .gitignore
├── app.json
├── Dockerfile
├── index.js
├── package-lock.json
├── package.json
├── Procfile
├── project-structure.txt
├── README.md
├── test.js
├── public
│ ├── lang-logo.png
│ ├── node.svg
│ └── stylesheets
│ └── main.css
└── views
  ├── pages
  │ ├── db.ejs
  │ └── index.ejs
  └── partials
  ├── header.ejs
  └── nav.ejs
```

---

## 🚀 CI/CD Pipeline Overview

This project is continuously deployed using a Jenkins pipeline running on an AWS EC2 instance.

✅ Steps:

1. Push to GitHub triggers a webhook
2. Jenkins pulls the latest code
3. Docker image is built from the Dockerfile
4. Existing container is stopped and removed
5. New container is deployed on port 3000

---

## 📄 Jenkinsfile

This file defines the pipeline:

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'node-app'
        CONTAINER_NAME = 'node-app-container'
        PORT = '3000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/tommarcusbruts/turbo-octo-bassoon.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                    dockerImage.run("-d -p ${PORT}:${PORT} --name ${CONTAINER_NAME}")
                }
            }
        }
    }

    post {
        always {
            echo "Build complete. Deployed to http://13.203.97.18:${PORT}/"
        }
    }
}

```
------
## 🐳 Dockerfile

```bash
FROM node:18-alpine

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "app.js"]
```
---
## 🔗 GitHub Webhook

A GitHub webhook is configured to POST to Jenkins on each push:

- URL: http://<jenkins-public-ip>:8080/github-webhook/
- Event: Just the push event
- Jenkins auto-builds the pipeline

---

## 🧱 How to Reproduce This Setup

1. Create an AWS EC2 Ubuntu instance
2. Install Jenkins and Docker
3. Add the Jenkins user to the docker group
4. Configure security groups to allow inbound traffic on:
   - Port 8080 (for Jenkins Web UI)
   - Port 3000 (for the running Node.js app)
   - Port 443 (for GitHub webhook POST request)
5. Create a Jenkins pipeline job pointing to this GitHub repository
6. Set up a GitHub webhook to point to Jenkins:
   - URL: http://<your-jenkins-ec2-ip>:8080/github-webhook/
   - Trigger: Just the push event

---

## 🧪 Test It

To test the full CI/CD pipeline:

1. Make any change in the repository and push to GitHub
2. Jenkins should automatically trigger the pipeline
3. Once the build is complete, visit your app at:

   👉 http://<ec2-instance-ip>:3000/
