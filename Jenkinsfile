pipeline {
    agent any

    environment {
        IMAGE_NAME = 'bonyubgu/dev01'
        TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bonyubking/dev01.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(
			credentialsId: 'dockerhub-creds', 
			usernameVariable: 'DOCKER_USER', 
			passwordVariable: 'DOCKER_PASS'
		)]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh "docker build -t $IMAGE_NAME:$TAG ."
                        sh "docker push $IMAGE_NAME:$TAG"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl set image deployment/dev01-deploy dev01=$IMAGE_NAME:$TAG"
            }
        }
    }
}

