pipeline{
    
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/punit1407/two-tier-flask-app/", "master")
               }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t punit-app ."
            }
            
        }
        stage("Test"){
            steps{
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerHubCreds","punit-app")
                }  
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }

post{
        success{
            script{
                emailext from: 'punit117aws@gmail.com',
                to: 'punit117aws@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'punit117aws@gmail.com',
                to: 'punit117aws@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
