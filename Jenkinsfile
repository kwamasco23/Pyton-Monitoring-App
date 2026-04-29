pipeline {
    agent any

    environment {
        IMAGE = "kwamasco/basicmonitoringapp4k8s"
        TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/kwamasco23/Pyton-Monitoring-App.git'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t $IMAGE:$TAG ."
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh "docker login -u $USER -p $PASS"
                    sh "docker push $IMAGE:$TAG"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl set image deployment/monitoring-app monitoring-app=$IMAGE:$TAG
                """
            }
        }
    }
}
