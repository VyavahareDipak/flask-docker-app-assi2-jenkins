pipeline {
    agent any

    environment {
        IMAGE_NAME = 'dipak018/flask-app-ass2-img:latest'
        CONTAINER_NAME = 'flask-container'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch:"main" , url:'https://github.com/VyavahareDipak/flask-docker-app-assi2-jenkins.git'
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
stage('Push to Docker Hub') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'docker-access-token', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASSWORD%"
}

        script {
            bat "echo Pushing Docker Image: %IMAGE_NAME%"
            def pushStatus = bat(returnStatus: true, script: "docker push %IMAGE_NAME%")
            if (pushStatus != 0) {
                error("❌ Docker push failed! Check Docker Hub permissions.")
            }
        }
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
            bat 'docker logout'
        }
    }
}
