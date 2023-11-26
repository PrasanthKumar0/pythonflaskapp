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
                    // Checkout the code from the Git repository
                    git branch: 'main', url: GIT_REPO
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install python3-venv package
                    sh 'sudo -S apt-get update'
                    sh 'sudo -S apt-get install -y python3-venv'

                    // Create virtual environment
                    sh 'python3 -m venv venv'

                    // Activate virtual environment and install dependencies
                    sh 'bash -c "source venv/bin/activate && pip install -r ./requirements.txt"'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Set correct permissions for the private key
                    sh "chmod 400 ${SSH_KEY}"

                    // Copy files to EC2 instance using SCP
                    sh "scp -i ${SSH_KEY} -r * ${EC2_USER}@${EC2_INSTANCE}:${DEPLOY_PATH}"

                    // SSH into EC2 and restart the application (replace with your own command)
                    sh "ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_INSTANCE} 'cd ${DEPLOY_PATH} && source venv/bin/activate && python your_app.py'"
                }
            }
        }

        stage('SSH into EC2') {
            steps {
                script {
                    // Set correct permissions for the private key
                    sh "chmod 400 ${SSH_KEY}"

                    // SSH into EC2 for additional steps (replace with your own commands)
                    sh "ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_INSTANCE} 'cd ${DEPLOY_PATH} && source venv/bin/activate && your_additional_command_here'"
                }
            }
        }
    }
}
