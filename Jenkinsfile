pipeline {  

    agent any 

    stages{  

         stage("code"){  

             steps {  

                 git url: "https://github.com/LondheShubham153/two-tier-flask-app.git" , branch: "master"  

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
                 sh "mkdir -p mysql-data && chmod 777 mysql-data"
                 sh "docker compose up -d --build"  

             }  

         }    

     }          

}  
