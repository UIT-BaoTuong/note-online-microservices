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
        stage('Update Manifests') {
            agent {
                docker { image 'alpine/git' }
            }
            steps {
                script {
                    sh """
                        git config --global user.email 'tuongndb@gmail.com'
                        git config --global user.name 'Jenkins Bot'
                    """
                    sh "sed -i 's|image: ${DOCKER_HUB_USER}/frontend:.*|image: ${DOCKER_HUB_USER}/frontend:${env.BUILD_NUMBER}|g' k8s-manifest/frontend.yaml"
                    sh "sed -i 's|image: ${DOCKER_HUB_USER}/user-service:.*|image: ${DOCKER_HUB_USER}/user-service:${env.BUILD_NUMBER}|g' k8s-manifest/user-service.yaml"
                    sh "sed -i 's|image: ${DOCKER_HUB_USER}/note-service:.*|image: ${DOCKER_HUB_USER}/note-service:${env.BUILD_NUMBER}|g' k8s-manifest/note-service.yaml"
                    sh "sed -i 's|image: ${DOCKER_HUB_USER}/noteonline-postgres:.*|image: ${DOCKER_HUB_USER}/noteonline-postgres:${env.BUILD_NUMBER}|g' k8s-manifest/postgres.yaml"

                    withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh """
                            git add k8s-manifest/*.yaml
                            git commit -m "chore: update image tags to build ${env.BUILD_NUMBER} [skip ci]"
                            git push https://${GIT_PASSWORD}@github.com/UIT-BaoTuong/note-online-microservices.git HEAD:main
                        """
                    }
                }
            }
        }
    }
}