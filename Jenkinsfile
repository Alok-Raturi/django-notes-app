@Library('shared-library') _
pipeline{
    agent{
        label 'slave'
    }
    
    stages{
        stage("Code"){
            steps{
                script{
                    gitClone("https://github.com/Alok-Raturi/django-notes-app.git","test")
                }
            }
        }
        stage("Build"){
            steps{
                echo  "For building the code"
                sh "docker build -t notes-app:latest ."
                echo "Builded the images"
            }
        }
        stage("Test"){
            steps{
                echo "For testing the code"
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "This is pushing the image to Docker hub"
                withCredentials([usernamePassword(
                    credentialsId:"dockerCred",
                    passwordVariable:"password",
                    usernameVariable:"username"
                )]){
                    sh "docker login -u ${env.username} -p ${env.password}"
                    sh "docker image tag notes-app:latest ${env.username}/notes-app:latest"
                    sh "docker push ${env.username}/notes-app:latest"
                    
                }
            }   
        }
        stage("Deploy"){
            steps{
                echo "For deploying the code"
                sh 'docker compose up -d'
            }
        }
    }
}
