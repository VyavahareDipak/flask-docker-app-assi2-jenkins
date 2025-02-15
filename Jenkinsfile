pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myusername/flask-app:latest'
        CONTAINER_NAME = 'flask-container'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-username/flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Run Tests') {
            steps {
                bat "docker run --rm $IMAGE_NAME pytest"
            }
        }

        stage('Pubat to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-token', variable: 'DOCKER_PASSWORD')]) {
                    bat "echo $DOCKER_PASSWORD | docker login -u myusername --password-stdin"
                }
                bat "docker pubat $IMAGE_NAME"
            }
        }

        stage('Deploy Application') {
            steps {
                bat "docker stop $CONTAINER_NAME || echo not runnig"
                bat "docker rm $CONTAINER_NAME || echo error in removing container"
                bat "docker run -d -p 5000:5000 --name $CONTAINER_NAME $IMAGE_NAME"
            }
        }
    }

    post {
        always {
            bat 'docker ps -a'
        }
    }
}
