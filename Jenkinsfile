pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-web-app"
        DOCKER_HUB_USER = "your-dockerhub-username"
        DOCKER_HUB_PASS = credentials('docker-hub-creds') // Add Docker Hub creds in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/my-web-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_HUB_USER%/%IMAGE_NAME%:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                bat "echo %DOCKER_HUB_PASS% | docker login -u %DOCKER_HUB_USER% --password-stdin"
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                bat "docker push %DOCKER_HUB_USER%/%IMAGE_NAME%:latest"
            }
        }

        stage('Deploy Container') {
            steps {
                bat "docker stop %IMAGE_NAME% || exit 0"
                bat "docker rm %IMAGE_NAME% || exit 0"
                bat "docker run -d -p 7070:80 --name %IMAGE_NAME% %DOCKER_HUB_USER%/%IMAGE_NAME%:latest"
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Visit http://localhost:7070"
        }
    }
}
