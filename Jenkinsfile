pipeline {
    agent any
    environment {
        IMAGE_NAME = "my-app"
        IMAGE_TAG = "latest"
    }
    
   

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }    
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").pull()
                }
            }
        }

  
        stage('Deploy Docker Container') {
           steps {
                script {
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").run("-d -p 8080:8080 --name my-container")
                }
            }
        }
    }
        

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
