pipeline {
    agent any
    
    environment {
        // Define the version number as Jenkins' build number
        VERSION = "1.0.${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '1b1573bd-4499-4083-81a8-9baa677de4a8', url: 'https://github.com/BarakAgranov/bynet.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker images using Docker Compose
                    sh "docker-compose build --build-arg VERSION=${VERSION}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to DockerHub
                    sh "docker login --username agranov9 --password IdkfaIddqd9088!"
                    
                    // Tag the built image with the version before pushing
                    sh "docker tag agranov9/backend-todo:latest agranov9/backend-todo:${VERSION}"
                    
                    // Tag the built image with the version before pushing
                    sh "docker tag agranov9/frontend-todo:latest agranov9/frontend-todo:${VERSION}"
                    
                    // Push the images using Docker Compose
                    sh "docker-compose push"
                    
                    // No need to logout here, but if you want to ensure you're logged out at the end:
                    sh "docker logout"
                }
            }
        }
    }
}
