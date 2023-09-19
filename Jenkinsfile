pipeline {
    agent any

    stages {
        stage("code") {
            steps {
                echo "Copy the code"
                git url: "https://github.com/MKshah01/notes-app.git", branch: "main"
            }
        }
        stage("build") {
            steps {
                echo "Build the code"
                sh "docker build -t ${env.dockerhubuser}/notes-app:latest ."
            }
        }
        stage("push to docker hub") {
            steps {
                echo "Push the image"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/notes-app:latest"
                }
            }
        }
        stage("deploy") {
            steps {
                echo "Deploy the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
