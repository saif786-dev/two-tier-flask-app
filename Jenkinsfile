pipeline {
    agent { label "dev" }

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/saif786-dev/two-tier-flask-app.git", branch: "master"
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
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
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
