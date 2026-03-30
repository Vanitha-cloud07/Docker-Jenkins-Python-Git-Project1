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
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
                sh 'docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:latest'
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }

        stage('Pull Latest Image') {
            steps {
                sh 'docker pull $IMAGE_NAME:latest'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name $CONTAINER_NAME $IMAGE_NAME:latest'
            }
        }

        stage('Verify Application') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost:5000'
            }
        }
    }
}