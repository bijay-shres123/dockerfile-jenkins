pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-docker-image"
        CONTAINER_NAME = "my-docker-container"
        PORT = "5000"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image '${IMAGE_NAME}'..."
                    sh """
                    docker build -t ${IMAGE_NAME} .
                    """
                    echo "Docker image '${IMAGE_NAME}' built successfully."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "Stopping and removing any existing container '${CONTAINER_NAME}' (if exists)..."
                    // Ensure no conflicting containers are running
                    sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    """
                    
                    echo "Running Docker container '${CONTAINER_NAME}' on port ${PORT}..."
                    sh """
                    docker run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${IMAGE_NAME}
                    """
                    echo "Docker container '${CONTAINER_NAME}' is running."
                }
            }
        }

        stage('Container Logs') {
            steps {
                script {
                    echo "Fetching logs from Docker container '${CONTAINER_NAME}'..."
                    sh """
                    docker logs ${CONTAINER_NAME}
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                echo "Cleaning up unused Docker resources to save space..."
                sh """
                docker image prune -f || true
                docker container prune -f || true
                """
            }
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs above for more details."
        }
    }
}

