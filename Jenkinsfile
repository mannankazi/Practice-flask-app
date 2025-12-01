pipeline {  

    agent any 

    stages{  

         stage("code"){  

             steps {  

                 git url: "https://github.com/mannankazi/Practice-flask-app.git" , branch: "master"  

             }  

         } 

         stage("build") {  

             steps{  

                 sh "docker build -t my-app . "  

             }  

         }  

         stage("test") {  

             steps{  

                 echo "test krne wala test code likh dega"  

             }  

         }  

         stage("Push to dockerHub"){ 

             steps{ 

                 withCredentials([usernamePassword( 

                     credentialsId:"DockerHubCredentials", 

                     passwordVariable: "DockerHubpassssworddd", 

                     usernameVariable: "DockerHubUserNameeee" 

                     )]){ 

                     sh "docker login -u ${env.DockerHubUserNameeee} -p ${env.DockerHubpassssworddd}" 

                     sh "docker image tag my-app ${env.DockerHubUserNameeee}/my-app:latest" 

                     sh "docker push ${env.DockerHubUserNameeee}/my-app:latest " 

                        } 

                  

             } 

              

         } 

         stage("Deploy") {  

             steps{  
                 sh "docker compose down -v --remove-orphans || true"
                 sh "docker compose build --no-cache my-app"
                 sh "docker compose up -d --build --no-cache my-app "  

             }  

         }    

     }          

}  
