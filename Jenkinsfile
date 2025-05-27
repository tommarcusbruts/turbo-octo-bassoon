pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nodejs-demo-app'
        CONTAINER_PORT = '3000'
        HOST_PORT = '3000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/tommarcusbruts/turbo-octo-bassoon.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker rm -f ${IMAGE_NAME} || true"
                    sh "docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${IMAGE_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Verify Container Running') {
            steps {
                sh "docker ps | grep ${IMAGE_NAME}"
            }
        }
    }

    post {
        always {
            echo "Build and deploy steps complete."
        }
    }
}
