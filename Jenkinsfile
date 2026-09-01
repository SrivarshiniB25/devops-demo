pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh '''
                    cd app
                    python -m pytest
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build -t devops-demo:${BUILD_NUMBER} .
                '''
            }
        }
    }
}
