pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'johnsonbv/java-web-calculator'
        DOCKER_CREDENTIALS_ID = 'johnsonbv-creds-id'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/JohnsonBV/JavaWeb3.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '/usr/bin/docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | /usr/bin/docker login -u "$DOCKER_USER" --password-stdin
                        /usr/bin/docker push $DOCKER_IMAGE
                    '''
                }
            }
        }
    }
}

