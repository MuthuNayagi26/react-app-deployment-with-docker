pipeline {
    agent any

    environment {
        IMAGE_NAME = "muthunayagi/react-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/MuthuNayagi26/react-app-deployment-with-docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop react || true
                docker rm react || true
                docker run -d -p 5000:80 --name react $IMAGE_NAME
                '''
            }
        }
    }
}

