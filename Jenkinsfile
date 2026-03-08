pipeline {
    agent any

    environment {
        IMAGE_NAME = "reshma0654/sevenwonders"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/Reshma-0654/Wonders.git'
            }
        }

        stage('Install Node Modules') {
            agent {
                docker {
                    image 'node:18'
                }
            }
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:3000 $IMAGE_NAME'
            }
        }

    }
}
