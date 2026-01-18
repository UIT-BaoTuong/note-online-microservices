pipeline {
    agent {
        label 'kaniko' 
    }

    environment {
        DOCKER_HUB_USER = 'tuongndb1609'
        IMAGE_NAME      = 'noteonline-postgres'
        IMAGE_TAG       = "${env.BUILD_NUMBER}"
        CONTEXT_PATH    = "postgres"
        DOCKERFILE_PATH = "postgres/Dockerfile"
    }

    stages {
        stage('Build and Push with Kaniko') {
            steps {
                container('kaniko') {                     
                    sh """
                    /kaniko/executor \
                        --context ${env.WORKSPACE}/${env.CONTEXT_PATH} \
                        --dockerfile ${env.WORKSPACE}/${env.DOCKERFILE_PATH} \
                        --destination ${env.DOCKER_HUB_USER}/${env.IMAGE_NAME}:${env.IMAGE_TAG} \
                        --destination ${env.DOCKER_HUB_USER}/${env.IMAGE_NAME}:latest \
                        --cache=true
                    """
                }
            }
        }
    }
}