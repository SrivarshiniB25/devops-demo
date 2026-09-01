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

                    python3 -m venv venv

                    venv/bin/pip install --upgrade pip

                    venv/bin/pip install -r requirements.txt

                    venv/bin/pytest
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
