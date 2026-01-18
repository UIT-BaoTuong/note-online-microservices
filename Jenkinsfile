pipeline {
    agent {
        label 'docker-build' 
    }
    stages {
        stage('Build Docker Image') {
            steps {
                container('docker') { 
                    dir('postgres') {
                        sh 'docker build -t note-postgres-image:latest .'
                    }
                }
            }
        }
    }
}