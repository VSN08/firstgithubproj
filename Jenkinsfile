pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url : https://github.com/VSN08/firstgithubproj.git, branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                # e.g., ./gradlew build or npm install
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                # e.g., ./gradlew test or npm test
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the application..."
                # Add your deployment commands
            }
        }
    }
}
