@Library("shared") _
pipeline{
    agent {label "dev"};
    stages{
        stage("Code clone"){
            steps{
                script{
                    clone("https://github.com/sitansudas/two-tier-flask-app-prac.git","master")
                }
            }
        }

        stage("Trivy file system scan"){
            steps{
                script{
                    trivy()
                }
            }
        }

        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test"){
            steps{
                echo "Developer / Tester test likhenge ..."
            }
        }

        stage("Push to Docker-hub"){
            steps{
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }
            }
        }

        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }

post {
    script{
        email_text("sdjgaming008@gmail.com")
    }
}
    
}
