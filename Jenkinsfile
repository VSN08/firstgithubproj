pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_PATH = './docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/VSN08/firstgithubproj.git'
            }
        }

        stage('Build & Deploy') {
            steps {
                script {
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} up --build -d'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Running tests..."
                    // Add test steps here if needed
                }
            }
        }

        stage('Clean up') {
            steps {
                script {
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} down'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up resources...'
            sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} down'
        }
    }
}
