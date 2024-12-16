pipeline {
    agent any
    stages {
        stage('Debugging') {
            steps {
                echo 'Debugging pipeline execution. Running updated Jenkinsfile...'
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Starting Docker build...'
                    sh 'docker build -t my-docker-image .'
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    echo 'Running the built image...'
                    sh 'docker run -d -p 5000:5000 my-docker-image'
                }
            }
        }
    }
}
