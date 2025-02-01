pipeline {
    agent any

    environment {
        IMAGE_NAME = "myimage"
        DOCKER_TLS_VERIFY = "1"
        DOCKER_HOST = "tcp://127.0.0.1:4039"
        DOCKER_CERT_PATH = "C:\\Users\\Ganesh.R\\.minikube\\certs"
        MINIKUBE_ACTIVE_DOCKERD = "minikube"
        KUBECONFIG = "C:\\Users\\Ganesh.R\\.kube\\config"
        MINIKUBE_HOME = 'C:\\minikube'
        MINIKUBE_CMD = 'minikube'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stages {
            stage('Start Minikube') {
                steps {
                    script {
                        bat """
                        %MINIKUBE_CMD% start --driver=docker
                        """
                    }
                }
            }

        stage('Set Docker and kubectl Context') {
            steps {
                script {
                    powershell """
                        & minikube -p minikube docker-env --shell powershell | Invoke-Expression
                        kubectl config use-context minikube
                    """
                }
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

        stage('Load Image into Minikube') {
            steps {
                script {
                    powershell """
                        minikube image load ${IMAGE_NAME}:latest
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
