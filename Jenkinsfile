pipeline {
    agent any

    environment {
        IMAGE_NAME = "harshithpenmetsa/container-app"
        IMAGE_TAG = "v1"
        DOCKER_EXE = "C:\\Users\\harsh\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe"
        KUBECTL_EXE = "C:\\Users\\harsh\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe"
        KUBECONFIG = "C:\\Users\\harsh\\.kube\\config"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Project checked out from GitHub'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"%DOCKER_EXE%" build -t %IMAGE_NAME%:%IMAGE_TAG% .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-pat',
                    variable: 'DOCKER_PAT'
                )]) {
                    bat '''
                    echo %DOCKER_PAT% | "%DOCKER_EXE%" login -u harshithpenmetsa --password-stdin
                    "%DOCKER_EXE%" push %IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                set KUBECONFIG=%KUBECONFIG%
                "%KUBECTL_EXE%" apply -f deployment.yaml
                "%KUBECTL_EXE%" apply -f service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '''
                set KUBECONFIG=%KUBECONFIG%
                "%KUBECTL_EXE%" get pods
                "%KUBECTL_EXE%" get services
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed!'
        }
    }
}