pipeline {

    agent any

    environment {
        GIT_URL = 'https://github.com/Diksha7876/11_105_CTRL-ALT-DEFEAT.git'
        BRANCH = 'main'
    }

    stages {

        stage('Debug Docker') {
            steps {
                sh '''
                    whoami
                    pwd

                    which docker || true
                    which docker-compose || true

                    docker --version || true
                    docker compose version || true
                    docker-compose --version || true
                '''
            }
        } 

        stage('Checkout Source') {
            steps {
                git branch: "${BRANCH}",
                    url: "${GIT_URL}"
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh 'docker compose down --remove-orphans || true'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build --pull --no-cache'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker compose ps'
                sh 'docker ps'
            }
        }
    }

    post {
        always {
            sh 'docker image prune -f || true'
        }

        success {
            echo 'Deployment completed successfully.'
        }

        failure {
            echo 'Deployment failed.'
        }
    }
}