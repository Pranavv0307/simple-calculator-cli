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

    stage('Docker Push') {
    steps {
        script {
            IMAGE_NAME = "calculator-image"
            DOCKERHUB_REPO = "pranav0307/calculator-image"
        }
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'USER', 
            passwordVariable: 'PASS'
        )]) {
            sh "echo $PASS | docker login -u $USER --password-stdin"
            sh "docker tag ${IMAGE_NAME} ${DOCKERHUB_REPO}:latest"
            sh "docker push ${DOCKERHUB_REPO}:latest"
        }
    }
}

}
