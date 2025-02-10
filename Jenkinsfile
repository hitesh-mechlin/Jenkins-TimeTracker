pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub-credentials')  // Jenkins credential id
        DOCKER_IMAGE = "hiteshmechlin/timetracker:tagone"  // Docker Hub repository and tag
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/hitesh-mechlin/Jenkins-TimeTracker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker Image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Docker login
                    sh "echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker Image to Docker Hub
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }
    }

    post {
        always {
            // Ensure that cleanWs is within a node block
            node {
                cleanWs()
            }
        }
    }
}
