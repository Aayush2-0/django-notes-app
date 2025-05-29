pipeline{

    agent any

    stages{ 

        stage("Code"){
            steps{
                echo "this is cloning the code"
                git url: "https://github.com/Aayush2-0/django-notes-app.git" , branch:"main"
                echo "Code Cloning Successfully"
            }
        }
        stage("Build"){
            steps{
                echo "this is building the code"  
                sh "docker build -t notes-app:latest ."              
            }
        }
        stage("Push to DockerHub"){
            steps{
                 echo "this is pushing image to the DockerHub"
                 withCredentials([usernamePassword(
                    'credentialsId':"dockerhubcred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                 sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                 sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                 sh "docker push ${env.dockerHubUser}/notes-app:latest" 
                 }                        
            }
        }
        stage("Deploy"){
            steps{
                echo "this is deploying the code"
                sh "docker-compose down && docker-compose up -d"                
            }
        }
    }
}
