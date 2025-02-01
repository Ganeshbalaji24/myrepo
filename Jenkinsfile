pipeline {
    agent any

    environment {
        // Define image name
        IMAGE_NAME = "myimage"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Set Minikube Docker Env') {
            steps {
                script {
                    // Point Docker to Minikube's Docker daemon
                    sh 'eval $(minikube docker-env)'
                }
            }
        }

        stage('Load Image into Minikube') {
            steps {
                script {
                    // Load the Docker image into Minikube
                    sh "minikube image load ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes manifests
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