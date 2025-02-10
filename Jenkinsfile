pipeline {
    agent any

    environment {
        // Define the Docker Hub repository and credentials
        DOCKER_REPO = "hiteshmechlin/timetracker"
        DOCKER_CREDENTIALS = "c68c6356-f22b-44ee-88ed-35cd9f8aade5"
        GITHUB_CREDENTIALS = "06073c04-ce3d-4b27-92a9-0d7932860dec"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout code from GitHub
                    git credentialsId: "${GITHUB_CREDENTIALS}", url: 'https://github.com/yourusername/yourrepo.git', branch: 'main'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_REPO}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        // Push the image to Docker Hub
                        docker.image("${DOCKER_REPO}:${env.BUILD_ID}").push()
                        docker.image("${DOCKER_REPO}:${env.BUILD_ID}").push("latest") // Optionally tag with 'latest'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after the build
        }
    }
}
