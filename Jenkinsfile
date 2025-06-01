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
                sshagent(['ec2-ssh-key-id']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@3.85.62.197 'rm -rf ~/therayu'
                    scp -o StrictHostKeyChecking=no -r . ec2-user@3.85.62.197:/home/ec2-user/therayu
                    """
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sshagent(['ec2-ssh-key-id']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@3.85.62.197 <<EOF
                      cd /home/ec2-user/therayu
                      docker-compose down || true
                      docker-compose up -d --build
                    EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Code changes deployed successfully."
        }
        failure {
            echo "❌ Deployment failed."
        }
    }
}
