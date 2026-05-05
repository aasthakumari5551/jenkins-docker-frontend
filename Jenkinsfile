pipeline {
    agent any

    environment {
        IMAGE = "my-app"
        CONTAINER = "my-app-container"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/aasthakumari5551/jenkins-docker-frontend.git'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Test') {
            steps {
                sh '''
                docker stop test || true
                docker rm test || true
                docker run -d -p 5000:80 --name test $IMAGE
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 5
                curl -f http://localhost:5000
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop $CONTAINER || true
                docker rm $CONTAINER || true
                docker run -d -p 80:80 --name $CONTAINER $IMAGE
                '''
            }
        }
    }
}