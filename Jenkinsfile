pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws')
        AWS_SECRET_ACCESS_KEY = credentials('aws')
        AWS_REGION            = 'ap-south-1'
        EC2_INSTANCE          = '3.108.236.212'
        KEY_PAIR_NAME         = '564832.pem'
        GITHUB_REPO_URL       = 'https://github.com/PrasanthKumar0/pythonflaskapp.git'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git GITHUB_REPO_URL
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh "ssh -o StrictHostKeyChecking=no -i /path/to/your/${KEY_PAIR_NAME} ec2-user@${EC2_INSTANCE} 'cd /path/to/your/app && git pull'"
                    sh "ssh -o StrictHostKeyChecking=no -i /path/to/your/${KEY_PAIR_NAME} ec2-user@${EC2_INSTANCE} 'cd /path/to/your/app && pip install -r requirements.txt'"
                    sh "ssh -o StrictHostKeyChecking=no -i /path/to/your/${KEY_PAIR_NAME} ec2-user@${EC2_INSTANCE} 'cd /path/to/your/app && python app.py'"
                }
            }
        }
    }
}
