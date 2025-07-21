pipeline {
    agent any // A default agent for stages that don't specify one.

    stages {
        stage('Build') {
            // Use a specific agent just for this stage.
            // This will pull a python:3.12-slim docker image and run the steps inside it.
            agent {
                docker { image 'python:3.12-slim' }
            }
            steps {
                sh 'pip install -r requirements.txt'
                echo 'Dependencies installed inside a Python container.'
            }
        }

        stage('Test') {
            // This stage will also run inside a python container
            agent {
                docker { image 'python:3.12-slim' }
            }
            steps {
                echo 'Running tests... (No tests defined for this example)'
                // In a real project, you'd run something like 'pytest' here
            }
        }

        stage('Build and Push Docker Image') {
            // This stage will use the default agent again, which is what we want,
            // as it's configured to talk to the Docker daemon.
            steps {
                script {
                    def dockerImage = docker.build("gouravmidya/fastapi-demo:${env.BUILD_NUMBER}")
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push()
                        echo "Docker image pushed to Docker Hub."
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished. Cleaning up workspace.'
            cleanWs()
        }
    }
}