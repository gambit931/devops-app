pipeline {
    agent any

    environment {
        IMAGE_NAME = "ysafdar/devops-app"
        IMAGE_TAG = "v1.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Tag Latest') {
            steps {
                sh 'docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }
}
