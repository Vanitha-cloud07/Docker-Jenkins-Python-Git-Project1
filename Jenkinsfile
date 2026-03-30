pipeline {
    agent any

    environment {
        IMAGE_NAME = "labubu67/my-flask-app"
        CONTAINER_NAME = "flask-project2"
        TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%TAG% .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %IMAGE_NAME%:%TAG%'
                bat 'docker tag %IMAGE_NAME%:%TAG% %IMAGE_NAME%:latest'
                bat 'docker push %IMAGE_NAME%:latest'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop %CONTAINER_NAME% || exit 0'
                bat 'docker rm %CONTAINER_NAME% || exit 0'
            }
        }

        stage('Pull Latest Image') {
            steps {
                bat 'docker pull %IMAGE_NAME%:latest'
            }
        }

        stage('Run New Container') {
            steps {
                bat 'docker run -d -p 5000:5000 --name %CONTAINER_NAME% %IMAGE_NAME%:latest'
            }
        }

        stage('Verify Application') {
            steps {
                bat 'timeout /t 5'
                bat 'curl http://localhost:5000'
            }
        }
    }
}