pipeline {
    agent any
    
    environment {
        // Define the version number as Jenkins' build number
        VERSION = "1.0.${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                git credentialsId: '1b1573bd-4499-4083-81a8-9baa677de4a8', url: 'https://github.com/BarakAgranov/bynet.git'
                
            }
        }

        stage('Build Docker Image') {
            steps {
                script {

                    // Build Docker images using Docker Compose
                    sh "docker compose build --no-cache --build-arg VERSION=${VERSION}"
                    
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to DockerHub
                    docker.withRegistry('https://index.docker.io/v1/', '9de0d131-8c0d-4e79-acc9-0fbfa3220f95') {

                    env.BACKEND_VERSION = "${VERSION}"
                    env.FRONTEND_VERSION = "${VERSION}"

                    sh "docker tag docker.io/agranov9/backend-todo:latest docker.io/agranov9/backend-todo:${VERSION}"
                    
                    sh "docker tag docker.io/agranov9/frontend-todo:latest docker.io/agranov9/frontend-todo:${VERSION}"

                    sh "docker push docker.io/agranov9/backend-todo:latest"
                    sh "docker push docker.io/agranov9/frontend-todo:latest"

                    }
                }
            }
        }
        stage('Checkout Kubernetes Manifests') {
            steps {
                git credentialsId: 'b87edbd6-746f-42b5-ba00-620c08622835', url: 'https://github.com/BarakAgranov/kubernetes-manifests.git'
                sh 'ls -lah'
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kubecon', variable: 'KUBECONFIG')]) {
                        sh 'kubectl apply -f ./staging/'
                        
                    }
                }
            }
        }
    }
}
