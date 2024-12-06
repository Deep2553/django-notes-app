pipeline {
    agent any 

    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Deep2553/django-notes-app.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building Docker image"
                sh "docker build -t django-project ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubPass", usernameVariable: "dockerhubUser")]) {
                    sh "docker tag django-project ${env.dockerhubUser}/django-project:latest"
                    sh "echo ${env.dockerhubPass} | docker login -u ${env.dockerhubUser} --password-stdin"
                    sh "docker push ${env.dockerhubUser}/django-project:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying with Docker Compose"
                sh """
                docker-compose down || true
                docker-compose up -d
                docker-compose ps
                """
            }
        }
    }

    post {
        always {
            echo "Cleaning up Docker resources"
            sh "docker system prune -f || true"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
