pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = 'dockerHub'  // Jenkins credentials ID for Docker Hub login
        DOCKER_IMAGE = 'ahmshaheer/flaskapp:latest' // Use your Docker Hub username and image name
    }

    stages {
        stage('Cloning the Code') {
            steps {
                git(
                    url: 'https://github.com/ahmshaheer/Flask-NotesApp-FullyFunctional.git', 
                    branch: 'main'
                )
            }
        }
        
        stage('Building the Code') {
            steps {
                script {
                    // Build Docker image using Dockerfile
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        
        stage('Running on ec2') {
            steps {
                script {
                    sh 'docker run -d -p 5000:5000 ahmshaheer/flaskapp:latest'
                }
            }
        }
    }
}
