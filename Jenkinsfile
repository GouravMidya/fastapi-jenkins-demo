pipeline {
    agent {
        kubernetes {
            // Inherit from the pod template named 'default' in your Jenkins config.
            // This is the modern, recommended approach.
            inheritFrom 'default' 
            defaultContainer 'docker'
        }
    }

    stages {
        stage('Build and Push Docker Image') {
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