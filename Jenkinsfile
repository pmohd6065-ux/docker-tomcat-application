pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "mohdparvez23/tomcat_app"
        CONTAINER_NAME = "tomcat-app"
    }

    stages {

        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  docker build \
                    -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                    -f tomcat/Dockerfile .
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh '''
                  echo ${DOCKERHUB_CREDENTIALS_PSW} | \
                  docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                  docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                  docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                  docker push ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                  docker stop ${CONTAINER_NAME} || true
                  docker rm ${CONTAINER_NAME} || true

                  docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 8081:8080 \
                    ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
