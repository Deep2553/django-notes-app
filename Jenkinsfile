pipeline {
    agent any 
    
    stages{
        stage("Code"){
            steps{
                 echo "Clonig the code"
                 git url:"https://github.com/Deep2553/django-notes-app.git", branch:"main"
            }
           
        }
        stage("build"){
            steps{
                 echo "Building image"
                 sh "docker build -t django-project ."
            }
        }
        
        stage("Push to Docker Hub"){
            steps{
                 echo "Depoying the container"
                 
                 withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubPass", usernameVariable:"dockerhubUser")]){
                 sh "docker tag django-project ${env.dockerhubUser}/django-project:latest"
                 sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                 sh "docker push ${env.dockerhubUser}/django-project:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                 echo "Deploy"
                 sh "docker-compose down && docker-compose up -d"
            }
            
        }
} 
}
