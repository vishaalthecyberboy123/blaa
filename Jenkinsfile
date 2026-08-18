pipeline {
    agent any

    environment {
        IMAGE_NAME = 'demo-ci-app'
        CONTAINER_NAME = 'demo-ci-container'
        HOST_PORT = '8080'
        CONTAINER_PORT = '80'
    }

    stages {

        stage('Check Source') {
            steps {
                sh '''
                    echo "===== PROJECT FILES ====="
                    ls -la

                    echo "===== CURRENT BRANCH ====="
                    git branch --show-current || true

                    echo "===== DOCKER ====="
                    docker --version
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                        -t ${IMAGE_NAME}:latest \
                        .
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${HOST_PORT}:${CONTAINER_PORT} \
                        ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify Website') {
            steps {
                sh '''
                    echo "===== CONTAINER ====="
                    docker ps

                    echo "===== WEBSITE TEST ====="
                    curl -I http://localhost:${HOST_PORT}
                '''
            }
        }
    }

    post {
        success {
            echo '================================='
            echo '✅ DEPLOYMENT SUCCESSFUL!'
            echo '🌐 http://YOUR_EC2_PUBLIC_IP:8080'
            echo '================================='
        }

        failure {
            echo '❌ DEPLOYMENT FAILED!'
            sh 'docker ps -a || true'
        }
    }
}
