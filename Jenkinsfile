pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = "hiteshmechlin/timetracker:tagone"
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
                    powershell 'docker buildx build -t $env:DOCKER_IMAGE .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    powershell 'echo $env:DOCKER_CREDENTIALS_PSW | docker login -u $env:DOCKER_CREDENTIALS_USR --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    powershell 'docker push $env:DOCKER_IMAGE'
                }
            }
        }
    }

    post {
        always {
            node('built-in') {
                cleanWs()
            }
        }
    }
}