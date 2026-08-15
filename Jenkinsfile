pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop jenkins-demo-container || true
                docker rm jenkins-demo-container || true
                docker run -d --name jenkins-demo-container -p 80:80 jenkins-demo
                '''
            }
        }
    }
}
