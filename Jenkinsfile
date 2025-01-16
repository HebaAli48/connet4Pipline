pipeline {
        agent any

    environment {
        DOCKER_IMAGE = 'hebaali4/react-bootstrap-app' // Docker image name
        DOCKER_TAG = 'latest' // Docker image tag
        DOCKER_CREDENTIALS = 'dockerhub' // Jenkins Docker Hub credentials ID
    }

    stages {
        stage('Install') {
            steps {
                script {
                    echo 'Installing dependencies of the React app...'
                    sh '''
                        npm install
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'building the React app...'
                    sh '''
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
