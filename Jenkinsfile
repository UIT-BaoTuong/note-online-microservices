//
pipeline {
    agent {
        label 'kaniko' 
    }

    environment {
        DOCKER_HUB_USER = 'tuongndb1609'
        IMAGE_NAME = 'noteonline-postgres'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build and Push') {
            steps {
                container('kaniko') { 
                    sh """
                    /kaniko/executor --context \$(pwd)/postgres \
                    --dockerfile \$(pwd)/postgres/Dockerfile \
                    --destination ${env.DOCKER_HUB_USER}/${env.IMAGE_NAME}:${env.IMAGE_TAG} \
                    --destination ${env.DOCKER_HUB_USER}/${env.IMAGE_NAME}:latest
                    """
                }
            }
        }
    }
}