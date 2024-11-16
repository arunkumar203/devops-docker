pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the repository
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker images for backend and frontend
                    sh '''
                        # Build backend image
                        docker build -t project-backend ./server

                        # Build frontend image
                        docker build -t project-frontend ./client
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run backend and frontend containers for testing
                    sh '''
                        # Run backend container
                        docker run -d -p 3000:3000 --name test-backend project-backend

                        # Run frontend container (served statically)
                        docker run -d -p 5000:5000 --name test-frontend project-frontend

                        # Wait for containers to start
                        sleep 10

                        # Test backend health
                        curl -f http://localhost:3000 || (echo "Backend test failed!" && exit 1)

                        # Test frontend availability
                        curl -f http://localhost:5000 || (echo "Frontend test failed!" && exit 1)

                        # Stop and remove containers after testing
                        docker stop test-backend test-frontend
                        docker rm test-backend test-frontend
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up Docker images if needed
                    sh '''
                        docker rmi project-backend
                        docker rmi project-frontend
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build, tests, and cleanup completed successfully!'
        }
        failure {
            echo 'Build, tests, or cleanup failed.'
        }
    }
}
