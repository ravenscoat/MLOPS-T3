pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials'
        IMAGE_NAME = 'my-docker-image'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout repository') {
            steps {
                // Checkout the Git repository
                checkout scm
            }
        }

        stage('Set up Docker Buildx') {
            steps {
                script {
                    // Create Docker Buildx builder and set it as the default
                    def buildxSetup = bat(script: 'docker buildx create --use', returnStatus: true)
                    if (buildxSetup != 0) {
                        echo 'Buildx already exists or failed to create. Continuing...'
                    }
                }
            }
        }

        stage('Log in to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        bat """
                            echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                        """
                    }
                }
            }
        }

        stage('Build and push Docker image') {
            steps {
                script {
                    def buildStatus = bat(script: "docker build -t %DOCKER_USERNAME%/${IMAGE_NAME}:${DOCKER_TAG} .", returnStatus: true)
                    if (buildStatus == 0) {
                        bat "docker push %DOCKER_USERNAME%/${IMAGE_NAME}:${DOCKER_TAG}"
                    } else {
                        error "Docker build failed with status ${buildStatus}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        failure {
            echo 'Pipeline execution failed.'
        }
    }
}
