pipeline {
    agent any

    environment {
        IMAGE_NAME = "myimage"
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

        stage('Set Minikube Docker Env') {
            steps {
                script {
                    // Set Minikube Docker environment variables for Docker Desktop
                    // Ensure you're using PowerShell to invoke Minikube environment variables
                    bat """
                    minikube start --driver=docker
                    minikube docker-env | Invoke-Expression
                    "
