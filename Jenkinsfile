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
                script {
                    // Use Windows batch commands
                    bat 'docker build -t flask-app:%BUILD_ID% .'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    // Use Windows batch commands with correct syntax
                    bat 'docker run -d --name flask-test -p 5001:5000 flask-app:%BUILD_ID%'
                    bat 'timeout 5'  // Windows equivalent of sleep
                    bat 'curl http://localhost:5001 || exit 1'
                    bat 'docker stop flask-test'
                    bat 'docker rm flask-test'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Use Windows batch commands
                    bat 'docker stop flask-production 2>nul || echo Container not running'
                    bat 'docker rm flask-production 2>nul || echo Container not found'
                    bat 'docker run -d --name flask-production -p 5000:5000 flask-app:%BUILD_ID%'
                }
            }
        }
    }
}
