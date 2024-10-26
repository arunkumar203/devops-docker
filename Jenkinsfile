pipeline {
    agent any
    
    environment {
        NODE_HOME = tool name: 'NodeJS' // Ensure Node.js is configured in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Step 1: Checkout code from the repository
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Step 2: Install Node.js dependencies
                sh 'npm install'
            }
        }
        
        stage('Build with Maven') {
            steps {
                // Step 3: Build the application with Maven
                // Make sure Maven is installed in Jenkins
                sh 'mvn clean install'
            }
        }
        
        stage('Run Selenium Tests') {
            steps {
                // Step 4: Run Selenium tests
                // Ensure Selenium is correctly set up (drivers, configurations)
                sh 'mvn test -Dtest=YourSeleniumTestClass'  // Replace with your test class if needed
            }
        }
    }
    
    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
            // Additional notifications, e.g., Slack or email, can be added here
            // mail to: 'your-email@example.com', subject: 'Build Failed', body: 'Please check the Jenkins job.'
        }
    }
}
