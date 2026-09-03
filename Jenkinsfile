pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "college-event"
    }

    stages {

        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Check Docker") {
            steps {
                sh 'docker --version'
                sh 'docker compose version'
            }
        }

        stage("Build") {
            steps {
                sh 'docker compose build'
            }
        }

        stage("Start Application") {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage("Check Containers") {
            steps {
                sh 'docker compose ps'
            }
        }

        stage("Health Check") {
            steps {
                sh 'curl -f http://localhost:5000/health'
            }
        }

        stage("Smoke Test") {
            steps {
                sh 'curl -f http://localhost:5000/'
            }
        }
    }

    post {
        always {
            sh 'docker compose logs --no-color > docker-logs.txt || true'
            archiveArtifacts artifacts: 'docker-logs.txt', allowEmptyArchive: true
        }

        success {
            echo 'College Event Registration application deployed successfully.'
        }

        failure {
            sh 'docker compose ps || true'
            echo 'Deployment failed. Check the console log and docker-logs.txt.'
        }
    }
}
