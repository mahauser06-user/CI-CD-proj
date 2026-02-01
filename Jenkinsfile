pipeline {
    agent any

    environment {
        IMAGE = "maha006/demo-app"
    }

    stages {

        stage('Checkout'){
            steps{
                git '<https://github.com/mahauser06-user/CI-CD-proj.git>'
            }
        }

        stage('Test'){
            steps{
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }

        stage('SonarQube'){
            steps{
                withSonarQubeEnv('SonarQube'){
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=demo \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=<sqa_beced08756d893a5325ce437d5031a3ebdf56790>
                    '''
                }
            }
        }

        stage('Docker Build'){
            steps{
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Docker Push'){
            steps{
                withCredentials([usernamePassword(
                credentialsId:'dockerhub-creds',
                usernameVariable:'USER',
                passwordVariable:'PASS')]){
                    
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $IMAGE'
                }
            }
        }

        stage('Deploy'){
            steps{
                sh 'docker run -d -p 5000:5000 $IMAGE'
            }
        }
    }
}
