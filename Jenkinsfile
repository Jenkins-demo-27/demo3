pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = ''
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    sh 'git clone "https://github.com/Jenkins-demo-27/demo3.git"'
                }
            }
        }

        stage('Extract Image Info') {
            steps {
                script {
                    // Change directory to the repository
                    dir('demo3') {
                        // Get the latest commit message
                        env.DOCKER_IMAGE_NAME = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                        // Print out for debugging
                        echo "Docker Image Name: ${env.DOCKER_IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.DOCKER_IMAGE_NAME) {
                        sh "docker build -t ${env.DOCKER_IMAGE_NAME} ."
                    } else {
                        error "DOCKER_IMAGE_NAME is not set. Please ensure the commit message contains the image name."
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    if (env.DOCKER_IMAGE_NAME) {
                        sh "docker run -d --name ${env.DOCKER_IMAGE_NAME}-container ${env.DOCKER_IMAGE_NAME}"
                    } else {
                        error "DOCKER_IMAGE_NAME is not set. Cannot run Docker container."
                    }
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
