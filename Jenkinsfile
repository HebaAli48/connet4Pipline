pipeline {
    agent any

    // environment {
    //     NODE_HOME = tool name: 'NodeJS', type: 'NodeJS'
    // }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git 'https://github.com/HebaAli48/simpleRestaurentPipeline.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests (you can add your testing framework here, e.g., Jest, Mocha)
                    sh 'npm test'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the app (if applicable)
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the app (this depends on your deployment strategy)
                    sh 'npm run deploy'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up any temporary files
                    sh 'npm run clean'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Something went wrong. Please check the build logs!'
        }
    }
}
