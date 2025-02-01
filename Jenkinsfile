pipeline {
    agent any

    environment {
        IMAGE_NAME = "myimage"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Set Minikube Docker Env') {
            steps {
                script {
                    sh 'eval $(minikube docker-env)'
                }
            }
        }

        stage('Load Image into Minikube') {
            steps {
                script {
                    sh "minikube image load ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f kubernetes/deployment.yaml"
                    sh "kubectl apply -f kubernetes/service.yaml"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded! Application deployed to Minikube."
        }
        failure {
            echo "Pipeline failed. Check the logs for errors."
        }
    }
}
