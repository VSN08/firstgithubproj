pipeline {
    agent any

    environment {
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
    }
}
