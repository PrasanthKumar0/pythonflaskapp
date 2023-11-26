pipeline {
    agent any

    environment {
        SSH_KEY = '/home/ubuntu/564832.pem'
        EC2_INSTANCE = 'ec2-3-108-236-212.ap-south-1.compute.amazonaws.com'
        EC2_USER = 'ubuntu'
        DEPLOY_PATH = '/home/ubuntu/pythonflaskapp'
        GIT_REPO = 'https://github.com/PrasanthKumar0/pythonflaskapp.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Install Dependencies') {
    steps {
        script {
            // Install python3-venv package
            sh 'sudo -S apt-get update && sudo -S apt-get install -y python3-venv'

            // Create virtual environment and install dependencies
            sh 'python3 -m venv venv'
            sh 'source venv/bin/activate && pip install -r requirements.txt'
        }
    }
}


        stage('Deploy to EC2') {
            steps {
                script {
                    // Copy files to EC2 instance using SCP
                    sh "scp -i ${SSH_KEY} -r * ${EC2_USER}@${EC2_INSTANCE}:${DEPLOY_PATH}"

                    // SSH into EC2 and restart the application
                    sh "ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_INSTANCE} 'cd ${DEPLOY_PATH} && source venv/bin/activate && python your_app.py'"
                }
            }
        }
    }
}
