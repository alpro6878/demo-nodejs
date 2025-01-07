pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    docker.build("my-app:latest", ".")
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                // Add test scripts or mock test commands
                sh 'docker run --rm my-flask-app python -c "import app; print(app.home())"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                    sh """ '
                    docker pull my-app:latest &&
                    docker stop my-app || true &&
                    docker rm my-app || true &&
                    docker run -d --name my-app -p 8080:8080 my-app:latest '
                    """
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
