pipeline {
    agent any

    environment {
        SSH_KEY = '/home/ubuntu/564832.pem'
        EC2_INSTANCE = 'ec2-13-233-163-83.ap-south-1.compute.amazonaws.com'
        EC2_USER = 'ubuntu'
        DEPLOY_PATH = '/home/ubuntu/pythonflaskapp'
        GIT_REPO = 'https://github.com/PrasanthKumar0/pythonflaskapp.git'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', url: GIT_REPO
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y python3-venv'

                    sh 'python3 -m venv venv'
                    sh 'bash -c "source venv/bin/activate && pip install -r ./requirements.txt"'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh "sudo chown ubuntu:ubuntu ${SSH_KEY}"
                    sh "sudo chmod 400 ${SSH_KEY}"

                    sh "scp -i ${SSH_KEY} -r * ${EC2_USER}@${EC2_INSTANCE}:${DEPLOY_PATH}"

                    sh "ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_INSTANCE} 'cd ${DEPLOY_PATH} && source venv/bin/activate && python your_app.py'"
                }
            }
        }

        stage('SSH into EC2') {
            steps {
                script {
                    sh "sudo chown ubuntu:ubuntu ${SSH_KEY}"
                    sh "sudo chmod 400 ${SSH_KEY}"

                    sh "ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_INSTANCE} 'cd ${DEPLOY_PATH} && source venv/bin/activate && your_additional_command_here'"
                }
            }
        }
    }
}
