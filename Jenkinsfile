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

        stage("Build") {
            steps {
                bat "docker compose build"
            }
        }

        stage("Start Application") {
            steps {
                bat "docker compose up -d"
            }
        }

        stage("Health Check") {
            steps {
                bat "docker compose ps"
                bat "curl.exe -f http://localhost:5000/health"
            }
        }

        stage("Smoke Test") {
            steps {
                bat "curl.exe -f http://localhost:5000/"
            }
        }
    }

    post {
        always {
            bat "docker compose logs --no-color > docker-logs.txt"
            archiveArtifacts artifacts: "docker-logs.txt", allowEmptyArchive: true
        }
        success {
            echo "College Event Registration application deployed successfully."
        }
        failure {
            bat "docker compose ps"
            echo "Deployment failed. Check the console log and docker-logs.txt."
        }
    }
}
