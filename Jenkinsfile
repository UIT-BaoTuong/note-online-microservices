pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'tuongndb1609' // Thay bằng ID Docker Hub của bạn
        IMAGE_NAME = 'noteonline-postgres'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Build Postgres Image') {
            steps {
                script {
                    dir('postgres') {
                        echo "Building Postgres Image..."
                        sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo \$PASS | docker login -u \$USER --password-stdin"
                        sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                        echo "Push successful!"
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up local images..."
                sh "docker rmi ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} || true"
            }
        }
    }
}