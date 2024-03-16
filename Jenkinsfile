pipeline{
    agent any
    
    stages{
        stage("Code Cloning"){
            steps{
                echo "Cloning the Code"
                git url:"https://github.com/Vishwaradhya91/django-notes-app.git", branch:"main"
            }
        }
        
        stage("Code Build"){
            steps{
                echo "Building the Code"
                sh "docker build -t django-notes-app ."
            }
        }
        
       stage("Dockerhub Login and Push"){
           steps{
               echo "Pushing image to Dockerhub"
               withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
               sh "docker tag django-notes-app ${env.dockerHubUser}/django-notes-app:V2"
               sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
               sh "docker push ${env.dockerHubUser}/django-notes-app:V2"
                   
               }
           }
       }
        
        
        stage("Deploy"){
            steps{
                echo "Deploying the Containers"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
