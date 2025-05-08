pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_PATH = "./docker-compose.yml"
        TEST_URL = "http://nginx_proxy" // use Docker service name instead of localhost
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build & Deploy') {
            steps {
                script {
                    echo 'Stopping old containers if any...'
                    sh 'docker rm -f flask_app nginx_proxy || true'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} down || true'

                    echo 'Building Docker images...'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} build --no-cache'

                    echo 'Starting containers...'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} up -d'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'sleep 10'

                    def response = sh(script: "curl -s ${TEST_URL}", returnStdout: true).trim()
                    echo "Received response: ${response}"

                    if (!response.contains("Hello from devops!")) {
                        error("Expected response not found! Got: ${response}")
                    }
                }
            }
        }

        stage('Clean up') {
            steps {
                script {
                    echo 'Cleaning up...'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} down || true'
                }
            }
        }
    }
}
