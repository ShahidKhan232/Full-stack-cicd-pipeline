pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ShahidKhan232/Therayu-cicd-pipeline.git', branch: 'main'
            }
        }

        stage('Transfer Latest Code to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh-key-id']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@44.202.156.216 'rm -rf ~/therayu && mkdir -p ~/therayu'
                        scp -o StrictHostKeyChecking=no -r . ec2-user@44.202.156.216:/home/ec2-user/therayu
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sshagent(credentials: ['ec2-ssh-key-id']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@44.202.156.216 <<EOF
                          cd /home/ec2-user/therayu
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
