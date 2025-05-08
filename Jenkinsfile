pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'darshanbhere7/abstergo-web:latest'
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/darshanbhere7/IseDevops.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop static-site || true'
                    sh 'docker rm static-site || true'
                    sh 'docker run -d --name static-site -p 8081:80 $DOCKER_IMAGE'
                }
            }
        }
    }
}
