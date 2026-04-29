pipeline {

    agent any

    environment {
        DOCKER_IMAGE = "kwamasco/basicmonitoringapp4k8s"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/kwamasco23/Pyton-Monitoring-App.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE:latest ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                    sh "docker push $DOCKER_IMAGE:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl rollout restart deployment monitoring-app"
            }
        }
    }
}
