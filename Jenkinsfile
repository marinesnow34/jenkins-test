pipeline {
    agent any

    environment {
        GHCR_URL = 'ghcr.io'
        IMAGE_NAME = 'ghcr.io/marinesnow34/jenkins-test'
        SSH_USER = credentials('ARM_SSH_USER')
        SSH_HOST = credentials('ARM_SSH_HOST')
    }
    
    stages {
        stage('SSH and execute ls') {
            steps {
                withCredentials([
                    string(credentialsId: 'ARM_SSH_HOST', variable: 'HOST')
                ]) {
                    sshagent(['arm-ssh-credential']) {
                        sh 'ssh -o StrictHostKeyChecking=no ${SSH_USER}@${HOST} "ls"'
                    }
                }
            }
        }
        // stage('Login to GHCR') {
        //     steps {
        //         script {
        //             docker.withRegistry("https://${env.GHCR_URL}", 'ghcr_credentials') { }
        //         }
        //     }
        // }
        // stage('Checkout') {
        //     steps {
        //         echo 'Checking out the repository...'
        //         checkout scm
        //     }
        // }
        // stage('Build') {
        //     steps {
        //         echo 'Building the application...'
        //         sh 'chmod +x gradlew'
        //         sh './gradlew clean build -x test'
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         echo 'Running tests...'
        //         sh './gradlew test'
        //     }
        // }
        // stage('Build Image') {
        //     steps {
        //         sh "docker build -t ${env.IMAGE_NAME}:latest ."
        //     }
        // }
        // stage('Push Image') {
        //     steps {
        //         script {
        //             docker.withRegistry("https://${env.GHCR_URL}", 'ghcr_credentials') {
        //                 docker.image("${env.IMAGE_NAME}:latest").push()
        //             }
        //         }
        //     }
        // }
    }
}
