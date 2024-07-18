pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/SakshamShrestha10/notes-test.git", branch: "main", credentialsId: 'git'
            }
        }
        stage("Build"){
            steps {
                echo "Building image"
                sh "docker build -t note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image..."
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"password",usernameVariable:"user")]){
                sh "docker tag note-app ${env.dockerHubUser}/note-app:latest"
                sh "docker login -u ${env.user} -p ${env.pass}"
                sh "docker push ${env.user}/note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the application"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
