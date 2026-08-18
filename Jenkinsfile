pipeline {
    agent any

    environment {
        IMAGE_NAME = 'demo-ci-app'
        CONTAINER_NAME = 'demo-ci-container'
    }

    stages {

        stage('Check Source') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t demo-ci-app:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker stop demo-ci-container || true
                    docker rm demo-ci-container || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                    --name demo-ci-container \
                    -p 8080:80 \
                    demo-ci-app:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    docker images
                    docker ps
                    curl -I http://localhost:8080
                '''
            }
        }
    }

    post {
        success {
            echo '✅ DEPLOYMENT SUCCESSFUL'
        }

        failure {
            echo '❌ DEPLOYMENT FAILED'
            sh 'docker ps -a || true'
        }
    }
}
