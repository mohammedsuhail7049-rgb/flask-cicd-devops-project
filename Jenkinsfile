pipeline {
    agent any

    environment {
        IMAGE_NAME = "mohammedsuhail7049/flask-app"
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Repository cloned successfully.'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker stop flask-container || true'
                sh 'docker rm flask-container || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --name flask-container -p 5000:5000 $IMAGE_NAME:$IMAGE_TAG'
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
