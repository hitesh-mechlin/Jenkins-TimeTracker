pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('c68c6356-f22b-44ee-88ed-35cd9f8aade5')  // Updated credentials ID
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
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push $DOCKER_IMAGE"
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
