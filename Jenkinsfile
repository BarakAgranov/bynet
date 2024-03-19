pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'agranov9/todo-list'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
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
                    // Build the Docker image
                    // docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", '.')
                    sh "docker-compose build"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker logout"
                    // Login to DockerHub
                    docker.withRegistry('https://index.docker.io/v1/', '9de0d131-8c0d-4e79-acc9-0fbfa3220f95') {
                        //Push the image
                        docker.image("backend-todo:latest").push()
                        
                    }
                    docker.withRegistry('https://index.docker.io/v1/', '9de0d131-8c0d-4e79-acc9-0fbfa3220f95') {
                        //Push the image
                        docker.image("frontend-todo:latest").push
                        
                    }
                }
            }
        }
    }
}
