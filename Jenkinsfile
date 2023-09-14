pipeline {
    agent any

    stages {
        stage('Pull Code from GitHub') {
            steps {
                // Checkout your application code from GitHub.
                git branch: 'main', url: ''
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Define variables for your Docker registry and image name.
                    def dockerRegistry = 'your-docker-registry'
                    def dockerImageName = 'your-docker-image-name'

                    // Build and tag the Docker image.
                    sh "docker build -t $dockerImageName ."

                    // Log in to your Docker registry (if needed).
                    sh "docker login -u your-docker-username -p your-docker-password $dockerRegistry"

                    // Push the Docker image to the registry.
                    sh "docker push $dockerRegistry/$dockerImageName"
                }
            }
        }

        stage('Deploy Application to Another VM') {
            steps {
                // Execute deployment steps on another VM. You can use SSH or any other method here.
                // For example, you can use SSH to copy the Docker image and start the container.
                script {
                    def remoteHost = 'your-vm-hostname'
                    def remoteUser = 'your-ssh-username'
                    def remotePassword = 'your-ssh-password'

                    sshagent(['your-ssh-credentials-id']) {
                        sh "ssh $remoteUser@$remoteHost 'docker pull $dockerRegistry/$dockerImageName'"
                        sh "ssh $remoteUser@$remoteHost 'docker run -d --name your-app-container $dockerRegistry/$dockerImageName'"
                    }
                }
            }
        }
    }
}
