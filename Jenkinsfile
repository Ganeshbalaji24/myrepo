pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkinsimage"
        MINIKUBE_DOCKER_ENV = "minikube -p minikube docker-env"
        KUBEVERSION = '1.21'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Use bat command to build Docker image in Windows
                    bat "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Load Image into Minikube') {
            steps {
                script {
                    // Load Docker image into Minikube's Docker daemon
                    bat "minikube image load ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes manifests
                    bat "kubectl apply -f deployment.yaml"
                    bat "kubectl apply -f service.yaml"
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
