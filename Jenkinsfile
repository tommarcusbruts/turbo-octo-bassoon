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

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker rm -f ${IMAGE_NAME} || true"
                    dockerImage.run("-d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${IMAGE_NAME}")
                }
            }
        }
    }

    post {
        always {
            echo "Build and deploy steps complete."
        }
    }
}
