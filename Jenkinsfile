pipeline {
    agent any
    
    stages {
        stage("Cloning") {
            steps {
                echo "cloning"
                git url:"https://github.com/harshau007/django-notes-app.git", branch:"main"
            }
        }
        stage("Building") {
            steps {
                echo "building"
                sh "docker build -t django-notes-app ."
            }
        }
        stage("Pushing to Dockerhub") {
            steps {
                echo "pushing"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerPass",usernameVariable:"dockerUser")]) {
                    sh "docker tag django-notes-app ${env.dockerUser}/django-notes-app:latest"
                    sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                    sh "docker push ${env.dockerUser}/django-notes-app:latest"
                }
            }
        }
        stage("Deploying") {
            steps {
                echo "deploying"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}