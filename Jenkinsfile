pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = 'dockerhub-creds'
        DOCKER_IMAGE = 'maha006/demo-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/mahauser06-user/CI-CD-proj.git', branch: 'main', credentialsId: 'github-creds'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Python dependencies
                bat 'python -m pip install --upgrade pip'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                // Run tests using pytest (or you can use unittest)
                bat 'pytest test_app.py'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        bat "docker push maha006/demo-app:latest"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                bat 'echo Deploying Python application...'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
