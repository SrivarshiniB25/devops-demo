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

                    rm -rf venv

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

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f devops-demo-container || true

                    docker run -d  --name devops-demo-container -p 4000:5000 devops-demo:${BUILD_NUMBER}
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    sleep 5

                    curl --fail http://host.docker.internal:4000/health
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
