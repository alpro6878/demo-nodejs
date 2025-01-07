pipeline {
    agent any

    env.VERSION = sh(returnStdout: true, script: "git describe --tags").trim()
    
   

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
                    docker.build("my-app:${env.VERSION}", ".")

                }
            }
        }

  
        stage('Deploy Docker Container') {
           steps {
            sh """
            ssh user@remote-server '
            docker pull my-app:${env.VERSION} &&
            docker stop my-app || true &&
            docker rm my-app || true &&
            docker run -d --name my-app -p 5000:5000 my-app:${env.VERSION}
            '
            """
        
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
