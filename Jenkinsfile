@Library("SharedLibs") _
pipeline {
    // agent any;
    agent {label "dev"};
    stages {
        stage("Code") {
            script {
                clone("https://github.com/bkmodi183/two-tier-flask-app.git", "main") 
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
    post{
        success{
            script{
                emailext from: 'friendclubabv@gmail.com',
                to: 'bkmodi183@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'friendclubabv@gmail.com',
                to: 'bkmodi183@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
