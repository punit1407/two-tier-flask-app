pipeline {
    agent { label "dev"};

    stages {

        stage('Code Clone') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t punits14/manish-app:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Developer / Tester tests likh ke dega...'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push punits14/manish-app:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build'
            }
        }
    }

    post {
        success {
            emailtext(
                to: 'punit117aws@gmail.com',
                subject: 'Build SUCCESS : Demo CICD App',
                body: 'Jenkins pipeline executed successfully'
            )
        }

        failure {
            emailtext(
                to: 'punit117aws@gmail.com',
                subject: 'Build FAILED : Demo CICD App',
                body: 'Jenkins pipeline failed . Please check logs.'
            )
        }
    }
}
