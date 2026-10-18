pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devinaaa/simple-app'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code berhasil diambil dari GitHub'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Test Container') {
            steps {
                sh 'docker run --rm $IMAGE_NAME:$BUILD_NUMBER python -c "from flask import Flask; print(\'Flask berhasil diimpor\')"'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: REGISTRY_CREDENTIALS,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh 'echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                    sh 'docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline selesai'
        }
    }
}
