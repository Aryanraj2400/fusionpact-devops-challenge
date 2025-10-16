pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') 
        DOCKERHUB_USER = 'aryanraj2400'
        FRONTEND_IMAGE = "${DOCKERHUB_USER}/fusionpact-devops-challenge-frontend:latest"
        BACKEND_IMAGE = "${DOCKERHUB_USER}/fusionpact-devops-challenge-backend:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Checking out code...'
                checkout scm
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                echo '🏗️ Building frontend image...'
                dir('frontend') {
                    sh 'docker build -t $FRONTEND_IMAGE:latest .'
                }
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                echo '🏗️ Building backend image...'
                dir('backend') {
                    sh 'docker build -t $BACKEND_IMAGE:latest .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo '🔐 Logging in to Docker Hub...'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                echo '🚀 Pushing images...'
                sh '''
                    docker push $FRONTEND_IMAGE:latest
                    docker push $BACKEND_IMAGE:latest
                '''
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo '📡 Deploying containers...'
                sh '''
                    docker-compose down
                    docker-compose pull
                    docker-compose up -d
                '''
            }
        }
    }

    post {
        always {
            echo '🧹 Cleaning up...'
            sh 'docker system prune -f'
        }
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
