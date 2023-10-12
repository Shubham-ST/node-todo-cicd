pipeline {
    agent { label "dev-agent" }
    stages{
        stage("code") {
            steps {
                echo "code cloning"
                git url: "https://github.com/Shubham-ST/node-todo-cicd.git" , branch: "master"
            }
        }
        stage("Build and Test") {
            steps {
                echo "building and testing"
                sh "docker build . -t node-todo-app"
            }
        }
        stage("Login and Push Image") {
            steps {
                 withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]) {
                     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-todo-app ${env.dockerHubUser}/node-todo-app:latest"
                    sh "docker push ${env.dockerHubUser}/node-todo-app:latest" 
                 }
            }
        }
        stage("Deploy") {
            steps {
                echo "deploying"
                sh "docker-compose down && docker-compose up -d"
               
            }
        }
    }
}
