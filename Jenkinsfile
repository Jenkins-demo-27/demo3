pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = ''
    }

    
    stages {
        stage('Checkout') {
            steps {
                sh '''
                git clone 'https://github.com/Jenkins-demo-27/demo3.git'
                cd demo3
                '''
            }
        }
        
        stage('Extract Image Innfo') {
            steps {
                script {
                    // Get the last codmmit message
                    #def commitMessage = sh(script: 'git log -1 --pretty=%B')

                    sh '''
                    DOCKER_IMAGE_NAME=$(git log -1 --pretty=%B)
                    '''
                    
                    // Extract image name and tag from the commit message
                    #def matcher = (commitMessage =~ /imageName:(\S+)\s*version:(\S+)/)
                    #if (matcher) {
                    #    DOCKER_IMAGE_NAME = matcher[0][1]
                    #    DOCKER_IMAGE_TAG = matcher[0][2]
                    #} #else {
                    #    error "Commit message does not contain the required 'imageName' and 'version' format."
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME} ."
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d --name ${DOCKER_IMAGE_NAME}-container ${DOCKER_IMAGE_NAME}"
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
