pipeline {
    agent any

    environment {
        // Ensure that these environment variables are defined
        DOCKER_COMPOSE_PATH = "./docker-compose.yml"
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
                    // Stop and remove containers if they exist
                    sh 'docker rm -f flask_app nginx_proxy || true'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} down || true'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} build --no-cache'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} up -d'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'

                    // Sleep to give the containers time to fully start
                    sh 'sleep 10'

                    // Test that the app is correctly serving the expected response
                    def output = sh(script: 'curl -s http://localhost', returnStdout: true).trim()

                    // Output the result for logging
                    echo "Received response: ${output}"

                    // Check if the output contains "Hello from DevOps"
                    if (!output.contains('Hello from DevOps')) {
                        error("Expected response not found! Got: ${output}")
                    }
                }
            }
        }

        stage('Clean up') {
            steps {
                script {
                    echo 'Cleaning up resources...'
                    sh 'docker-compose -f ${DOCKER_COMPOSE_PATH} down || true'
                }
            }
        }
    }
}


