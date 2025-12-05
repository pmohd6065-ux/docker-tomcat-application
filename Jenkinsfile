pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub-creds')
    }

    stages {
        stage('Build Docker Images') {
            steps {
                    sh 'docker-compose build'
                
            }
        }

        stage('Login to Docker Hub') {
            steps {
                    sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                    sh 'docker-compose push'
            }
        }

        stage('Run Containers') {
            steps {
                    sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful kid! Webapp running on port 8081, MySQL on port 3306.'
        }
        failure {
            echo '❌ Deployment failed idiot. Check Jenkins logs and docker-compose output you fool !!.'
        }
    }
}
