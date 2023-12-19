pipeline{
    agent any
    stages{
        stage('Code'){
            steps{
                git url: "https://github.com/Aditya241193/django_notes_app.git", branch: "main"
                sh "echo 'code pulling from github'"
            }
        }
        stage('Build'){
            steps{
                sh "docker build -t django-app:latest ."
                sh "echo 'code Building'"
            }
        }
        stage('push'){
            steps{
                withCredentials([usernamePassword(credentialsId: "dockerHUB" , passwordVariable: "DockerPass", usernameVariable: "DockerUser")])
                {
                sh "docker login -u ${env.DockerUser} -p ${env.DockerPass}"
                sh "echo 'image pushing to docker hub'"
                sh "docker tag django-app:latest ${env.DockerUser}/django-app:latest"
                sh "docker push ${env.DockerUser}/django-app:latest"
                }
            }
        }
        stage('deploy'){
            steps{
                sh "echo 'Deploying app on ec2 server'"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
