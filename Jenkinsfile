pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-ci-app"
        CONTAINER_NAME = "demo-ci-container"
    }

    stages {

        stage('Check Files') {
            steps {
                sh '''
                    echo "=== Files ==="
                    ls -la
                    echo "=== Docker ==="
                    docker --version
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 8080:80 \
                    ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    docker ps
                    curl -I http://localhost:8080
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
            echo '🌐 Open http://YOUR_EC2_PUBLIC_IP:8080'
        }

        failure {
            echo '❌ Deployment Failed!'
            sh 'docker ps -a || true'
        }
    }
}
