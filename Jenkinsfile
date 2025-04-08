pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'juhichoudhary/flask-hello-world:latest'
        KUBE_CONFIG = credentials('kubeconfig') // Jenkins credential ID for kubeconfig
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds' // Jenkins DockerHub creds ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
    sh 'docker push juhichoudhary/flask-hello-world:latest'
}

            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
