pipeline {
    agent any
    environment {
        IMAGE_NAME = "yourdockerhubuser/flask-cicd-example:${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-user/flask-cicd-example.git'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    docker.build(IMAGE_NAME)
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        docker.image(IMAGE_NAME).push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh '''
                    docker stop flask-app || true
                    docker rm flask-app || true
                    docker run -d --name flask-app -p 80:80 ${IMAGE_NAME}
                    '''
                }
            }
        }
    }
}
