pipeline {
    agent any

    environment {
        GHCR_URL = 'ghcr.io'
        IMAGE_NAME = 'ghcr.io/marinesnow34/jenkins-test'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the repository...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'chmod +x gradlew'
                sh './gradlew clean build -x test'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh './gradlew test'
            }
        }
        // stage('Login to GHCR') {
        //     steps {
        //         script {
        //             docker.withRegistry("https://${env.GHCR_URL}", 'ghcr_credentials') { }
        //         }
        //     }
        // }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry("https://${env.GHCR_URL}", 'ghcr_credentials') {
                        docker.image("${env.IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }
}
