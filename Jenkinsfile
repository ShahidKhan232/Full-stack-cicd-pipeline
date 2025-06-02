pipeline {
    agent any

    environment {
        REMOTE_USER = 'ec2-user'
        REMOTE_HOST = '98.80.76.194'
        REMOTE_PATH = '/home/ec2-user/fullstack-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ShahidKhan232/full-stack-cicd-pipeline.git', branch: 'main'
            }
        }

        stage('Transfer Latest Code to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key-id', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        chmod 600 $SSH_KEY
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST 'rm -rf $REMOTE_PATH && mkdir -p $REMOTE_PATH'
                        scp -i $SSH_KEY -o StrictHostKeyChecking=no -r . $REMOTE_USER@$REMOTE_HOST:$REMOTE_PATH
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key-id', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        chmod 600 $SSH_KEY
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST <<EOF
                            cd $REMOTE_PATH
                            docker-compose down || true
                            docker-compose up -d --build
                        EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Code changes deployed successfully."
        }
        failure {
            echo "Deployment failed."
        }
    }
}
