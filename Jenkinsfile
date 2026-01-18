pipeline {
    agent any 

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'cd postgres && docker build -t note-postgres-image:latest .'
                }
            }
        }
        
        stage('Verify Image') {
            steps {
                sh 'docker images | grep note-postgres-image'
            }
        }
    }
}