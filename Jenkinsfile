pipeline {
    agent any

    environment {
        IMAGE_NAME = "for-vs-code-app"
        CONTAINER_NAME = "for-vs-code-container"
        NEXUS_URL = "3.110.106.18:8082"
        NEXUS_REPO = "docker-repo"
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
                sh """
                docker build -t $IMAGE_NAME .
                """
            }
        }

        stage('Tag Image for Nexus') {
            steps {
                sh """
                docker tag $IMAGE_NAME $NEXUS_URL/$NEXUS_REPO/$IMAGE_NAME:latest
                """
            }
        }

        stage('Login to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    docker login $NEXUS_URL -u $USER -p $PASS
                    """
                }
            }
        }

        stage('Push to Nexus') {
            steps {
                sh """
                docker push $NEXUS_URL/$NEXUS_REPO/$IMAGE_NAME:latest
                """
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
                sh """
                docker run -d \
                --name $CONTAINER_NAME \
                -p 80:80 \
                $IMAGE_NAME
                """
            }
        }

        stage('Check Running Containers') {
            steps {
                sh 'docker ps'
            }
        }
    }
}
