pipeline {
    agent any

    environment {
        IMAGE_NAME = "for-vs-code-app"
        CONTAINER_NAME = "for-vs-code-container"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Vasu-dhara-12/FOR_VS_CODE.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 80:80 \
                $IMAGE_NAME
                '''
            }
        }

        stage('Check Running Containers') {
            steps {
                sh 'docker ps'
            }
        }
    }
}