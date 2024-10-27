pipeline {
    agent any
    stages {
        stage('github-clone') {
            steps {
                git branch: 'main', credentialsId: 'jenkins', url: '{REPOSITORY URL}'
            }
        }
        
   		// stage...
   	}
}
