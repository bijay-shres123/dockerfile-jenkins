pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t my-docker-image .'
                }
            }
        }
    }
}
