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
                 // copy the env file into our directory
                sh "cp ~/jobs/env_files/mpesa_payment_gateway_env/.env ."

                // Build our current image
                sh "docker build -t ${IMG_NAME}:${IMG_REVISION}-${BUILD_ID} ."
            }
        }
 
        stage('Deploying') {
            agent any
            steps {
                dir(path: env.BUILD_ID) {
                    sh "docker tag ${IMG_NAME}:${IMG_REVISION}-${BUILD_ID} ${IMG_NAME}:latest"
                    sh "docker-compose up -d"
                }
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
                dir(path: env.BUILD_ID) {
                    sh "docker tag ${IMG_NAME}:${IMG_REVISION}-${BUILD_ID} ${IMG_NAME}:latest"
                    sh "docker-compose up -d"
                  rtDockerPush(
                      serverId: "jFrog-ar1",
                      image: "${IMG_NAME}:${IMG_REVISION}-${BUILD_ID} ${IMG_NAME}:latest",
                      host: 'https://roytech.jfrog.io/',
                      targetRepo: 'local-repo', // where to copy to (from docker-virtual)
                      // Attach custom properties to the published artifacts:
                      properties: 'project-name=docker1;status=stable'
                  )
                }
            }
            post {
                success {
                    echo 'Deployment to Jfrog successful'
                }
            }
        }
    }
}

                
