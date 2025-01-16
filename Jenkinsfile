pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim' // Use Node.js Docker image for building and testing
            args '-p 3000:3000' // Map port for local testing
        }
    }

    environment {
        CI = 'true' // Enable continuous integration environment
        DOCKER_IMAGE = 'hebaali4/react-bootstrap-app' // Docker image name
        DOCKER_TAG = 'latest' // Docker image tag
        DOCKER_CREDENTIALS = 'dockerhub' // Jenkins Docker Hub credentials ID
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Installing dependencies and building the React app...'
                    sh '''
                        npm install
                        npm run build
                    '''
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

        // stage('Deliver') {
        //     steps {
        //         script {
        //             echo 'Delivering the application...'
        //             sh './jenkins/scripts/deliver.sh'
        //             input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //             sh './jenkins/scripts/kill.sh'
        //         }
        //     }
        // }

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
            echo 'Pipeline completed successfully. The Docker image has been pushed to Docker Hub.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
