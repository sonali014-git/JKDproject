pipeline {
    agent any

    environment {
        IMAGE_NAME = "sonalidocker11/cicd-demo"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push sonalidocker11/cicd-demo:latest
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
