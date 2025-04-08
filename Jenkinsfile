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
                    app = docker.build("flask-app:${env.BUILD_ID}")
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    sh 'docker run --rm flask-app:${BUILD_ID} pytest'
                    
                    // Test the running app
                    sh 'docker run -d --name flask-test -p 5001:5000 flask-app:${BUILD_ID}'
                    sh 'sleep 5'
                    sh 'curl http://localhost:5001 || exit 1'
                    sh 'docker stop flask-test'
                    sh 'docker rm flask-test'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop flask-production || true'
                    sh 'docker rm flask-production || true'
                    sh 'docker run -d --name flask-production -p 5000:5000 flask-app:${BUILD_ID}'
                }
            }
        }
    }
}