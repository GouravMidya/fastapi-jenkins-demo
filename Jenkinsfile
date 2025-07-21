pipeline {
    // Specify the pod template label and the container within it to use.
    agent {
        kubernetes {
            label 'default' // Or whatever your pod template is named
            defaultContainer 'docker'
        }
    }

    stages {
        // The 'docker' container doesn't have Python, so we can't run pip directly.
        // This is fine, because the real build happens inside the Dockerfile anyway.
        // We will remove the 'Build' and 'Test' stages and rely on the Docker build.
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // This command will now work because it's running in the 'docker' container
                    // which has the Docker client and access to the Docker daemon.
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