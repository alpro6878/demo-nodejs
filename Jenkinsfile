pipeline {
    agent any
    environment {
        IMAGE_NAME = "my-app"
        IMAGE_TAG = "latest"
        GIT_CREDENTIALS = credentials('e1e33829-f4d7-485f-8857-944526831580')  // Use the credentials ID you defined

    }
    
   

    stages {
        stage('Checkout') {
             steps {
                // Checkout from GitHub repository using the stored credentials
                git credentialsId: 'e1e33829-f4d7-485f-8857-944526831580', url: 'https://github.com/alpro6878/demo-nodejs.git'
             }  
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}", ".")
                }
            }
        }
        
        stage('Clean up') {
            steps {
                script {
                    // Stop and remove any existing containers with the same name
                    sh 'docker ps -q --filter "name=my-container" | xargs -r docker stop'
                    sh 'docker ps -a -q --filter "name=my-container" | xargs -r docker rm'
                }
            }
        }
        
        stage('Deploy Docker Container') {
           steps {
                script {
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").run("-d -p 5000:5000 --name my-container")
                }
            }
        }

            
            
    }
}
