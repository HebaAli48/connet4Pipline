pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'Node12', type: 'NodeJS' // Node.js tool for building the app
        DOCKER_IMAGE = 'hebaali4/react-bootstrap-app'   // Docker image name
        DOCKER_TAG = 'latest'                           // Docker image tag
        DOCKER_CREDENTIALS = 'dockerhub'               // Jenkins Docker Hub credentials ID
    }

    tools {
        nodejs 'Node12' // Ensure this matches the configured NodeJS tool name
    }

    stages {
        stage('Verify Node.js') {
            steps {
                script {
                    echo "Using Node.js from: ${env.NODE_HOME}"
                    sh "${env.NODE_HOME}/bin/node --version" // Verify Node.js installation
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build React App') {
            steps {
                script {
                    echo 'Building the React app...'
                    sh 'npm run build'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    echo 'Building the Docker image...'
                    sh """
                        docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                    """
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    echo 'Pushing the Docker image to Docker Hub...'
                    withDockerRegistry([credentialsId: DOCKER_CREDENTIALS, url: '']) {
                        sh """
                            docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                        """
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    echo 'Cleaning up Docker images...'
                    sh """
                        docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG}
                        docker system prune -f
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build and push to Docker Hub successful!'
        }
        failure {
            echo 'Build failed. Please check the logs for details.'
        }
    }
}
