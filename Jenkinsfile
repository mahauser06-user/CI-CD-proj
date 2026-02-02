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
                bat 'python -m pip install --upgrade pip'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                bat 'pytest test_app.py'
            }
        }

        // ✅ SONARQUBE STAGE ADDED HERE
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                    sonar-scanner ^
                    -Dsonar.projectKey=CI-CD-proj ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.login=YOUR_TOKEN
                    """
                }
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
