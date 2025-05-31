pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = credentials('dockerhub-creds-id')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/your-org/your-repo.git', branch: 'main'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    sh """docker build -t shahidkhan232/frontend:latest ."""
                }
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    sh """docker build -t shahidkhan232/backend:latest ."""
                }
            }
        }

        stage('Push Docker Images to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push shahidkhan232/frontend:latest
                        docker push shahidkhan232/backend:latest
                    """
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key-id']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@54.167.116.168 <<EOF
                      docker pull shahidkhan232/frontend:latest
                      docker pull shahidkhan232/backend:latest

                      docker stop frontend backend || true
                      docker rm frontend backend || true

                      docker run -d --name frontend -p 80:3000 shahidkhan232/frontend:latest
                      docker run -d --name backend -p 5353:5353 shahidkhan232/backend:latest
                    EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo " Build and Deployment Successful"
        }
        failure {
            echo " Build or Deployment Failed"
        }
    }
}
