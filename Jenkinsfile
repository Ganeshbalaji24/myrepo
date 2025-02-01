pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkinsimage"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    // Check if the files exist in the workspace
                    sh 'ls -l'  // List files in the root of the workspace
                }
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
                    // Apply the deployment and service files from the root directory
                    sh "kubectl apply -f deployment.yaml"
                    sh "kubectl apply -f service.yaml"
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
