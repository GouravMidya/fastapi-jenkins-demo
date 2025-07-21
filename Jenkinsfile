pipeline {
    // 1. Agent Declaration: Run this pipeline on any available Jenkins agent.
    agent any

    // 2. Stages: The main blocks of work in the pipeline.
    stages {
        // 3. Checkout Stage: Clones the code from your repository.
        // stage('Checkout') {
        //     steps {
        //         // 'git' command checks out the code from the repo configured in the Jenkins job.
        //         git 'https://github.com/GouravMidya/fastapi-jenkins-demo.git'
        //         echo 'Source code checked out.'
        //     }
        // }

        // 4. Build Stage: Prepare the application (e.g., install dependencies).
        stage('Build') {
            steps {
                // This step could involve more complex build tasks if needed.
                // For Python, it's often as simple as ensuring dependencies can be installed.
                sh 'pip install -r requirements.txt'
                echo 'Dependencies installed.'
            }
        }

        // 5. Test Stage: Run automated tests.
        stage('Test') {
            steps {
                // For this simple example, we'll just echo a message.
                // In a real project, you would run your test suite here (e.g., 'pytest').
                echo 'Running tests... (No tests defined for this example)'
            }
        }

        // 6. Docker Build & Push Stage: Create and publish the Docker image.
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Use the Docker plugin to interact with Docker.
                    // Replace 'your-dockerhub-username' with your actual Docker Hub username.
                    // The BUILD_NUMBER is a variable provided by Jenkins.
                    def dockerImage = docker.build("gouravmidya/fastapi-demo:${env.BUILD_NUMBER}")

                    // To push, you first need to configure credentials in Jenkins.
                    // Go to Jenkins > Manage Jenkins > Credentials.
                    // Add a "Username with password" credential with your Docker Hub ID/password.
                    // Give it a descriptive ID, like 'dockerhub-credentials'.
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        // Push the image to Docker Hub.
                        dockerImage.push()
                        echo "Docker image pushed to Docker Hub."
                    }
                }
            }
        }
    }

    // 7. Post-build Actions: Actions to run after the pipeline completes.
    post {
        always {
            // Clean up the workspace to save disk space.
            echo 'Pipeline finished. Cleaning up workspace.'
            cleanWs()
        }
    }
}