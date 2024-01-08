pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dh_cred')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Init') {
            steps {
                sh 'echo Init Step'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Define the Docker image name with a tag
                    def dockerImageName = "$DOCKERHUB_CREDENTIALS_USR/client:$BUILD_ID"
                    
                    // Navigate to the frontend directory
                    dir('./frontend') {
                        // Build and push Docker image
                        sh "docker build -t $dockerImageName ."
                        sh "docker push $dockerImageName"
                    }

                    // Optionally, you can remove the local Docker image
                    sh "docker rmi $dockerImageName"
                }
            }
        }

        stage('logout') {
            steps {
                sh 'docker logout'
            }
        }
    }
}
