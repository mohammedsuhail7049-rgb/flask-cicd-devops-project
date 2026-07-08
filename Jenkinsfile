pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Repository cloned successfully.'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app:v1 .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker stop flask-container || true
                docker rm flask-container || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --name flask-container -p 5000:5000 flask-app:v1'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Executed Successfully!'
        }

        failure {
            echo 'Pipeline Failed!'
        }
    }
}
