pipeline {
    agent any

    environment {
        // Docker registry credentials (if pushing to Docker Hub)
        DOCKER_CREDENTIALS = 'dockerhub-creds' // replace with your Jenkins credentials ID
        DOCKER_IMAGE = 'mahauser06/demo-app'
        SONARQUBE_SERVER = 'SonarQube' // replace with your SonarQube server name in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Correct Git URL, no angle brackets
                git url: 'https://github.com/mahauser06-user/CI-CD-proj.git', branch: 'main'
            }
        }

        stage('Test') {
            steps {
                // Example: run Maven tests
                // Adjust this according to your project (Maven/Gradle/other)
                sh 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    // Run SonarQube scan
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Example: simple deployment command
                // Replace with your deployment logic (Kubernetes, SSH, etc.)
                sh 'echo "Deploying application..."'
            }
        }
    }

    post {
        always {
            cleanWs() // clean workspace after build
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
