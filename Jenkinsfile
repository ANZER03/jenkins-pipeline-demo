pipeline {
    agent any

    environment {
        IMAGE_NAME = "hello-world-nginx"
        CONTAINER_NAME = "hello-world-container"
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
                    if (docker ps -a -q -f name=${CONTAINER_NAME}) {
                        docker stop ${CONTAINER_NAME}
                        docker rm ${CONTAINER_NAME}
                    }
                    docker run -d --name ${CONTAINER_NAME} -p 8080:80 ${IMAGE_NAME}
                    """
                }
            }
        }
    }
}
