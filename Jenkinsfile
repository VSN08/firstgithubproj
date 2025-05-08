pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/VSN08/firstgithubproj.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo "Building the project"
                // e.g., ./gradlew build or npm install
                // Add your build steps here
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying the project"
                // Add your deployment steps here
            }
        }
    }
}

