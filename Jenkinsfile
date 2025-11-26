pipeline {
    agent any

    environment {
        GIT_BRANCH = "main"
        PATH = "/var/lib/jenkins/.local/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}",
                    credentialsId: 'github-cred',
                    url: 'https://github.com/Pranavv0307/simple-calculator-cli.git'
            }
        }

        stage('Install Requirements') {
            steps {
                sh 'python3 -m pip install --upgrade pip setuptools wheel'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    IMAGE_NAME = "calculator-image"
                }
                sh "docker build -t ${IMAGE_NAME} ."
                sh "docker images"
            }
        }
    }

    post {
        always {
            echo "Pipeline Completed: ${currentBuild.currentResult}"
        }
        failure {
            echo "Error: Build or Tests Failed"
        }
    }
}
