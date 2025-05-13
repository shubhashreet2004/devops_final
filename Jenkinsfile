pipeline {
    agent any

    environment {
        IMAGE_NAME = "shubhashree1311/my-login-page"
        IMAGE_TAG = "latest"
        DOCKER_USER = "shubhashree1311"  // Hardcoded Docker username
        DOCKER_PASS = "shubha@2004"      // Hardcoded Docker password
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Start Minikube') {
            steps {
                script {
                    sh 'minikube start --driver=docker'
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Hardcoded Docker login
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }

        stage('Push to Docker Registry') {
            steps {
                script {
                    sh """
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    docker logout
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Something went wrong."
        }
    }
}
