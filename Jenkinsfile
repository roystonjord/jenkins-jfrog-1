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
                sh "docker tag node-hello-world roytech.jfrog.io/default-docker-local/node-hello-world:latest"
                //sh "docker run -d -p 3000:3000 roytech.jfrog.io/default-docker-local/node-hello-world:latest --name hello-world"
                sh "docker images --filter dangling=true  -q | xargs docker rmi -f"
              
            }
            post {
                success {
                    echo 'Deployment successful'
                }
            }
        }
      
       stage('Connect to Jfrog') {
            agent any
            steps {
                
                echo 'Deployment successful'
                
                    rtServer (
                        id: 'RoyJfrog',
                        url: 'http://my-artifactory-domain/artifactory',
                        // If you're using username and password:
                        username: 'davidjosephsizya@gmail.com',
                        password: 'Jfrog@2021',
                        // If you're using Credentials ID:
                        //credentialsId: 'ccrreeddeennttiiaall',
                        // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
                        bypassProxy: true,
                        // Configure the connection timeout (in seconds).
                        // The default value (if not configured) is 300 seconds:
                        timeout: 300
                    )
                
                 
            }
        }
        
        stage('Deploying to Jfrog') {
            agent any
            steps {
                echo "nice try"
                
                  rtDockerPush(
                      serverId: "RoyJfrog",
                      image: "roytech.jfrog.io/default-docker-local/node-hello-world",
                      host: 'https://roytech.jfrog.io/',
                      //url: 'https://roytech.jfrog.io/artifactory',
                      targetRepo: 'default-docker-local', // where to copy to (from docker-virtual)
                      // Attach custom properties to the published artifacts:
                      properties: 'project-name=docker1;status=stable' 
                  )
             
            }
           
           
            post {
                success {
                    echo 'Deployment to Jfrog successful'
                }
            }
        }
 
    }
    
}
