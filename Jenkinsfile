pipeline {
    agent any

    environment {
        SSH_KEY = '/home/ubuntu/564832.pem'
        EC2_INSTANCE = '3.108.236.212'
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
                    // Create SSH directory if it doesn't exist
                    sh 'mkdir -p ~/.ssh'

                    // Update SSH known hosts
                    sh "ssh-keyscan -H ${EC2_INSTANCE} >> ~/.ssh/known_hosts"

                    // SCP files to EC2 instance
                    sh "scp -i ${SSH_KEY} -r Jenkinsfile requirements.txt venv ${EC2_USER}@${EC2_INSTANCE}:${DEPLOY_PATH}"

                    // SSH into EC2, activate virtual environment, and start the app
                    sh "ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_INSTANCE} 'cd ${DEPLOY_PATH} && source venv/bin/activate && python your_app.py'"
                }
            }
        }
    }
}
