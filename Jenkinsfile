echo 'Building and Pushing Docker Image'
pipeline {
    agent any  

    environment {
        DOCKERHUB_CREDENTIAL_ID = '51'
    }

    stages {
        stage('Build and Push Image') {
    steps {
        script {
            echo 'Building and Pushing Docker Image'
           
            dir('front') {
                sh 'docker build -t raniaazz/devops:latest .'

                withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIAL_ID, usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
                }

                sh 'docker push raniaazz/devops:latest'        
            }
        }
    }
}


        stage('Testing') {
            steps {
                script {
                   
                    sh 'npm test'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh 'rm -rf temporary_files'
                }
            }
        }
    }
}
