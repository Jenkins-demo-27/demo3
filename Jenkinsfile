pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = ''
        DOCKER_IMAGE_TAG = ''
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Jenkins-demo-27/demo3.git'
            }
        }
        
        stage('Extract Image Innfo') {
            steps {
                script {
                    // Get the last commit message
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    
                    // Extract image name and tag from the commit message
                    def matcher = (commitMessage =~ /imageName:(\S+)\s*version:(\S+)/)
                    if (matcher) {
                        DOCKER_IMAGE_NAME = matcher[0][1]
                        DOCKER_IMAGE_TAG = matcher[0][2]
                    } else {
                        error "Commit message does not contain the required 'imageName' and 'version' format."
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d --name ${DOCKER_IMAGE_NAME}-container ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
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
