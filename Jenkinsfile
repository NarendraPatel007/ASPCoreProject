pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'nip04/mydockerrepo/ASPCoreProject'  // Replace with your Docker image name
        DOCKER_TAG = 'b1'  // You can change this to a versioning strategy
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git 'https://github.com/NarendraPatel007/ASPCoreProject.git' // Replace with your repository URL
            }
        }

        stage('Restore') {
            steps {
                // Restore dependencies
                script {
                    sh 'dotnet restore'
                }
            }
        }

        stage('Build') {
            steps {
                // Build the application
                script {
                    sh 'dotnet build --configuration Release'
                }
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                script {
                    sh 'dotnet test --no-build --configuration Release'
                }
            }
        }

        stage('Publish') {
            steps {
                // Publish the application
                script {
                    sh 'dotnet publish --configuration Release --output ./publish'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    sh '''
                    docker build -t $DOCKER_IMAGE_NAME:$DOCKER_TAG .
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker image to a registry
                script {
                    sh '''
                    docker login -u nip04 -p xGhY585$1
                    docker push $DOCKER_IMAGE_NAME:$DOCKER_TAG
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the Docker container (this example assumes you're using Docker on the same host)
                script {
                    sh '''
                    docker stop AspCoreProject1 || true
                    docker rm AspCoreProject1 || true
                    docker run -d --name AspCoreProject1 -p 8085:80 $DOCKER_IMAGE_NAME:$DOCKER_TAG
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
