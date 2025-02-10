pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hiteshmechlin/timetracker:tagone"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the Git repository from GitHub
                git branch: 'master', url: 'https://github.com/hitesh-mechlin/Jenkins-TimeTracker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use 'docker build' with the correct context (e.g., the current directory)
                    powershell 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Docker Hub credentials used for login
                    withCredentials([usernamePassword(credentialsId: 'c68c6356-f22b-44ee-88ed-35cd9f8aade5', 
                                                      usernameVariable: 'DOCKER_USERNAME', 
                                                      passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Login to Docker Hub using the credentials
                        powershell """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        """
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the built Docker image to Docker Hub
                    powershell "docker push $DOCKER_IMAGE"
                }
            }
        }
    }

    post {
        always {
            // Cleanup workspace after pipeline finishes, ensuring no leftover files
            cleanWs()
        }
    }
}
