pipeline {
    agent none
    environment {
        IMG_REVISION="1.0.0"
        IMG_NAME='roytech.jfrog.io/default-docker-local/node-hello-world'
    }
    stages {
        stage('Building') {
            agent any
            steps {
                echo "Trial Echo"
                // Build our current image
                sh "docker build -t node-hello-world ."
            }
        }
        
        
        stage('Deploying') {
            agent any
            steps {
                sh "docker tag node-hello-world:latest roytech.jfrog.io/default-docker-local/node-hello-world"
                sh "docker run -d -p 3000:3000 node-hello-world --name node-hello-world"
              
            }
            post {
                success {
                    echo 'Deployment successful'
                }
            }
        }
      
       stage('Deploying to Jfrog') {
            agent any
            steps {
                echo "nice try"
//                   rtDockerPush(
//                       serverId: "jFrog-ar1",
//                       image: "roytech.jfrog.io/default-docker-local/node-hello-world",
//                       host: 'https://roytech.jfrog.io/',
//                       targetRepo: 'default-docker-local', // where to copy to (from docker-virtual)
//                       // Attach custom properties to the published artifacts:
//                       properties: 'project-name=docker1;status=stable' 
//                   )
             
            }
            post {
                success {
                    echo 'Deployment to Jfrog successful'
                }
            }
        }
 
    }
    
}
