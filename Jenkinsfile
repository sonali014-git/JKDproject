pipeline {
    agent any

    environment {
        IMAGE_NAME = "dockerhubusername/cicd-demo"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/username/cicd-jenkins-k8s.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                  credentialsId: 'dockerhub-creds',
                  usernameVariable: 'USER',
                  passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
