pipeline {
    agent any

    stages {

        stage('Code Clone') {
            steps {
                checkout scm
            }
        }

        stage('Trivy File System Scan') {
            steps {
                sh '''
                  trivy fs --exit-code 0 --severity LOW,MEDIUM .
                  trivy fs --exit-code 1 --severity HIGH,CRITICAL .
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t punit1407/punit-app:latest .'
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
                      docker push punit1407/punit-app:latest
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
            emailext(
                to: 'punit117aws@gmail.com',
                subject: 'Build SUCCESS : Demo CICD App',
                body: 'Jenkins pipeline executed successfully ✅'
            )
        }

        failure {
            emailext(
                to: 'punit117aws@gmail.com',
                subject: 'Build FAILED : Demo CICD App',
                body: 'Jenkins pipeline failed ❌. Please check logs.'
            )
        }
    }
}
