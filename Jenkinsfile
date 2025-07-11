pipeline {
    agent any

    stages {
        stage('clone the code from git hub') {
            steps {
                git url:"https://github.com/basantgit/django-notes-app.git", branch: "main"
            }
        }
        stage('Build the images') {
            steps {
                echo "Building the image"
                sh "docker build -t basant-note-app ."
            }
        }
        stage('push the image into docker hub') {
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"docker-crds",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag basant-note-app ${env.dockerHubUser}/basant-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/basant-note-app:latest"
                }
            }
        }
        stage('deploy note application in docker container') {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}
