pipeline {
    agent any

    environment {
        IMAGE_NAME = "hello-world-nginx"
        CONTAINER_NAME = "hello-world-container"
        HOST_PORT = "8081"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    powershell "docker build -t ${IMAGE_NAME} ."
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
