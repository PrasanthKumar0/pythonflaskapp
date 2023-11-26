pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Your Docker build command
                    sh 'docker build -t prasanthk8/hey-python-flask:0.0.1.RELEASE .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Your Docker run command
                    sh 'docker container run -d -p 3000:3000 prasanthk8/hey-python-flask:0.0.1.RELEASE'
                }
            }
        }
    }
}
