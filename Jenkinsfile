pipeline {
    agent any

    environment {
        MINIKUBE_HOME = 'C:\\minikube'
        MINIKUBE_CMD = 'minikube'
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

        stage('List Pods') {
            steps {
                script {
                    bat """
                    %MINIKUBE_CMD% kubectl -- get pods
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                bat """
                %MINIKUBE_CMD% stop
                """
            }
        }
    }
}
