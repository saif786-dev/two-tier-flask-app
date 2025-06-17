@libraray{“Shared”} _
pipeline {
    agent { label "dev" }

    stages {
        stage("Code") {
            steps {
                clone("https://github.com/saif786-dev/two-tier-flask-app.git", "master")
            }
        }
        stage("Trivy file system scan"){
            steps{
                script{
                    trivy_fs()
                }
            }    
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "tester will test"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }

    post {
        success {
            script {
                emailext from: "saifmundhekar786@gmail.com",
                    to: "saifmundhekar786@gmail.com",
                    body: "Build Success For Demo cicd app",
                    subject: "Build Success For Demo cicd app"
            }
        }

        failure {
            script {
                emailext from: "saifmundhekar786@gmail.com",
                    to: "saifmundhekar786@gmail.com",
                    body: "Build Failed For Demo cicd app",
                    subject: "Build Failed For Demo cicd app"
            }
        }
    }
}
