pipeline {
    agent any

    environment {
        IMAGE_NAME = "myimage"
        KUBECONFIG = "C:\\Users\\Ganesh.R\\.kube\\config"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    powershell """
                        docker build -t ${IMAGE_NAME}:latest .
                    """
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    powershell """
                        kubectl apply -f deployment.yaml --validate=false
                        kubectl apply -f service.yaml --validate=false
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
        }
        failure {
            echo "Pipeline failed. Check logs for errors."
        }
    }
}
