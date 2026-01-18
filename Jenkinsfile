pipeline {
    agent any 

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dir('postgres') {
                        docker.build("note-postgres-image:latest")
                    }
                }
            }
        }
    }
}