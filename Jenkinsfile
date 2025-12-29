pipeline {
    agent any

    environment {
        APP_NAME = 'web-app'
        REPO_URL = 'https://github.com/123MANAR/my-web-app.git'
        GIT_BRANCH = 'main'
    }

    stages {
        stage('Getting Repo files') {
            steps {
                git branch: "${GIT_BRANCH}", credentialsId: 'github', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${APP_NAME}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                        docker run -p 5000:5000 --name "${APP_NAME}"-"${GIT_BRANCH}"-${BUILD_NUMBER} -d ${APP_NAME}:${BUILD_NUMBER}
                        docker ps
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "Docker image built and container running successfully!"
        }
        failure {
            echo "Something went wrong!"
        }
    }
}
