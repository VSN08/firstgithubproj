pipeline {
    agent any

    environment {
        // This is the path to your docker-compose file in the repository
        DOCKER_COMPOSE_PATH = './docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository from Git
                git 'https://github.com/VSN08/firstgithubproj.git' // Replace with your repo URL
            }
        }

        stage('Build & Deploy') {
            steps {
                script {
                    // Use Docker Compose to build and deploy the app
                    sh "docker-compose -f ${DOCKER_COMPOSE_PATH} up --build -d"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Add any tests you want to run here
                    echo "Testing the app..."
                    // For example, you can run curl requests or any other test here
                }
            }
        }

        stage('Clean up') {
            steps {
                script {
                    // Optionally, you can stop and remove containers after the build
                    sh "docker-compose -f ${DOCKER_COMPOSE_PATH} down"
                }
            }
        }
    }

    post {
        always {
            // Clean up even if the pipeline fails
            echo 'Cleaning up resources...'
            sh "docker-compose -f ${DOCKER_COMPOSE_PATH} down"
        }
    }
}
