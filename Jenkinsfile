pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-docker-image" // Name of the Docker image to build
        CONTAINER_PORT = "5000"         // Port used inside the container
        HOST_PORT = "5000"              // Port exposed on the host
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out the latest code from Git...'
                git branch: 'main', url: 'https://github.com/bijay-shres123/dockerfile-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image...'
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running the Docker container...'
                script {
                    // Check if a container is already running and stop it before creating a new one
                    sh """
                    docker ps -q --filter "name=${DOCKER_IMAGE}" | grep -q . && docker stop ${DOCKER_IMAGE} || true
                    docker run -d --name ${DOCKER_IMAGE} -p ${HOST_PORT}:${CONTAINER_PORT} ${DOCKER_IMAGE}
                    """
                }
            }
        }

        stage('Verify Container') {
            steps {
                echo 'Verifying the running container...'
                script {
                    sh "docker ps | grep ${DOCKER_IMAGE} || echo 'Container is not running!'"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs above for details.'
        }
    }
}
