pipeline {
    agent any

    environment {
        COMPOSE_FILE = './docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/VSN08/firstgithubproj.git', branch: 'main'
            }
        }

        stage('Build & Deploy') {
            steps {
                script {
                    // Clean up any previously running containers
                    sh 'docker rm -f flask_app nginx_proxy || true'

                    // Tear down previous network if any
                    sh 'docker-compose -f $COMPOSE_FILE down || true'

                    // Build without cache to ensure changes are applied
                    sh 'docker-compose -f $COMPOSE_FILE build --no-cache'

                    // Start up the containers
                    sh 'docker-compose -f $COMPOSE_FILE up -d'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'sleep 5'  // Wait a bit for containers to be ready
                    sh 'curl -f http://localhost | grep "Hello from DevOps"'
                }
            }
        }

        stage('Clean up') {
            steps {
                script {
                    sh 'docker-compose -f $COMPOSE_FILE down'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up resources...'
            sh 'docker-compose -f $COMPOSE_FILE down || true'
        }
    }
}

