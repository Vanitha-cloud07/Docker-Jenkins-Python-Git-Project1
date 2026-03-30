pipeline {
    agent any

    environment {
        IMAGE_NAME = "labubu67/my-flask-app"
        TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Vanitha-cloud07/Docker-Jenkins-Python-Git-Project1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push %IMAGE_NAME%:%TAG%"
            }
        }

        stage('Deploy Container') {
            steps {
                bat "docker stop flask-container2 || exit 0"
                bat "docker rm flask-container2 || exit 0"
                bat "docker run -d -p 5000:5000 --name flask-container2 %IMAGE_NAME%:%TAG%"
            }
        }

        stage('Verify Application') {
            steps {
                bat "ping 127.0.0.1 -n 6 > nul"
                bat "curl http://localhost:5000"
            }
        }
    }
}