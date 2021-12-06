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
                // Build our current image
                echo "Hello World"
            }
        }
 
    }
}

                
