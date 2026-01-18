pipeline {
    agent {
        label 'kaniko' 
    }

    environment {
        DOCKER_HUB_USER = 'tuongndb1609'
    }

    stages {
        stage('Build Postgres') {
            when { changeset "postgres/**" }
            steps {
                container('kaniko') {
                    echo "--- Building Postgres ---"
                    sh """
                    /kaniko/executor --context ${WORKSPACE}/postgres \
                        --dockerfile ${WORKSPACE}/postgres/Dockerfile \
                        --destination ${DOCKER_HUB_USER}/noteonline-postgres:${env.BUILD_NUMBER} \
                        --destination ${DOCKER_HUB_USER}/noteonline-postgres:latest \
                        --cache=true --cleanup
                    """
                }
            }
        }

        stage('Build User Service') {
            when { changeset "user-service/**" }
            steps {
                container('kaniko') {
                    echo "--- Building User Service ---"
                    sh """
                    /kaniko/executor --context ${WORKSPACE}/user-service \
                        --dockerfile ${WORKSPACE}/user-service/Dockerfile \
                        --destination ${DOCKER_HUB_USER}/user-service:${env.BUILD_NUMBER} \
                        --destination ${DOCKER_HUB_USER}/user-service:latest \
                        --cache=true --cleanup
                    """
                }
            }
        }

        stage('Build Note Service') {
            when { changeset "note-service/**" }
            steps {
                container('kaniko') {
                    echo "--- Building Note Service ---"
                    sh """
                    /kaniko/executor --context ${WORKSPACE}/note-service \
                        --dockerfile ${WORKSPACE}/note-service/Dockerfile \
                        --destination ${DOCKER_HUB_USER}/note-service:${env.BUILD_NUMBER} \
                        --destination ${DOCKER_HUB_USER}/note-service:latest \
                        --cache=true --cleanup
                    """
                }
            }
        }

        stage('Build Frontend') {
            when { changeset "frontend/**" }
            steps {
                container('kaniko') {
                    echo "--- Building Frontend ---"
                    sh """
                    /kaniko/executor --context ${WORKSPACE}/frontend \
                        --dockerfile ${WORKSPACE}/frontend/Dockerfile \
                        --destination ${DOCKER_HUB_USER}/frontend:${env.BUILD_NUMBER} \
                        --destination ${DOCKER_HUB_USER}/frontend:latest \
                        --cache=true --cleanup
                    """
                }
            }
        }
    }
}