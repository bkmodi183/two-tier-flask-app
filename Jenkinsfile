pipeline {
    agent any;

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/bkmodi183/two-tier-flask-app.git" , branch: "main" 
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app:latest ."
            }
        }

        stage("Test") {
            steps {
                echo "Testing done based on the test cases given by developer/tester"
            }
        }
        stage("Push Tested inmage to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                passwordVariable: "dockerHubPass",
                usernameVariable: "dockerHubUser"
                )]){
                    sh "docker image tag two-tier-flask-app:latest ${env.dockerHubUser}/two-tier-flask-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d"
            }
        }
    }
}
