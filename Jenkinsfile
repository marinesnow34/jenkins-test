pipeline {
    agent any

    environment {
        GHCR_URL = 'ghcr.io'
        IMAGE_NAME = 'ghcr.io/marinesnow34/jenkins-test'
        SSH_USER = credentials('ARM_SSH_USER')
        SSH_HOST = credentials('ARM_SSH_HOST')
        GHCR_USERNAME = 'marinesnow34'
        GHCR_PASSWORD = credentials('ghcr_credentials')
    }
    
    stages {
        stage('SSH and execute ls') {
            steps {
                sshagent(['arm-ssh-credential']) {
                    sh 'ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST} "ls"'
                }
            }
        }
        stage('Login to GHCR') {
            steps {
                script {
                    docker.withRegistry("https://${env.GHCR_URL}", 'ghcr_credentials') { }
                }
            }
        }
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
        stage('Deploy on Server') {
            steps {
                sshagent(['arm-ssh-credential']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST} "
                    echo '${GHCR_PASSWORD}' | docker login ${GHCR_URL} -u ${GHCR_USERNAME} --password-stdin
                    docker pull ${IMAGE_NAME}:latest
                    docker stop app || true && docker rm app || true
                    docker run -d --name app ${IMAGE_NAME}:latest
                    "
                    """
                }
            }
        }
    }
}
