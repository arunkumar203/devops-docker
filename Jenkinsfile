pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'sudo docker build -t node-app .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Optionally, you can run tests here
                    // For example, using curl to check if the app is running
                    sh '''
                        docker run -d -p 3000:3000 --name test-app node-app
                        sleep 5 # Wait for the app to start
                        curl http://localhost:3000
                        docker stop test-app
                        docker rm test-app
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up Docker images if needed
                    sh 'docker rmi node-app'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
