```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'demo-ci-app'
        CONTAINER_NAME = 'demo-ci-container'
        HOST_PORT = '8081'
    }

    stages {

        stage('Check Source') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:80 \
                    ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    docker images
                    docker ps
                    sleep 3
                    curl -f -I http://localhost:${HOST_PORT}
                '''
            }
        }
    }

    post {
        success {
            echo '✅ DEPLOYMENT SUCCESSFUL'
            echo 'Website running on port 8081'
        }

        failure {
            echo '❌ DEPLOYMENT FAILED'
            sh 'docker ps -a || true'
        }
    }
}
```
