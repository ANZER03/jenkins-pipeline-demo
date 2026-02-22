pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "anzer0300/tp5-devops-jenkins"
        IMAGE_NAME = "hello-world-nginx"
        CONTAINER_NAME = "hello-world-container"
        HOST_PORT = "8081"
        DOCKER_HUB_CREDENTIALS_ID = "docker-hub-credentials"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    powershell "docker build -t ${IMAGE_NAME} ."
                    // Tag for Docker Hub
                    powershell "docker tag ${IMAGE_NAME} ${DOCKER_HUB_REPO}:latest"
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS_ID, passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        powershell "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                        powershell "docker push ${DOCKER_HUB_REPO}:latest"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    powershell """
                    \$container = docker ps -a -q -f name=${CONTAINER_NAME}
                    if (\$container) {
                        Write-Host "Stopping and removing existing container: ${CONTAINER_NAME}"
                        docker stop \$container
                        docker rm \$container
                    }
                    Write-Host "Starting new container: ${CONTAINER_NAME} on port ${HOST_PORT}"
                    docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:80 ${IMAGE_NAME}
                    """
                }
            }
        }
    }
}
