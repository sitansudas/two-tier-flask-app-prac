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
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
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
    success {
        emailext(
            subject: "Build Successful",
            body: "Good news! Your build was successful.",
            to: "sitansudas255@gmail.com"
        )
    }

    failure {
        emailext(
            subject: "Build Failed",
            body: "Bad news! Your build has failed.",
            to: "sitansudas255@gmail.com"
        )
    }
}    
    
}
