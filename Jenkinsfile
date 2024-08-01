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
                    dir('demo3') {
                        // No need to `cd`, `dir` block handles it
                    }
                }
            }
        }

        stage('Extract Image Info') {
            steps {
                script {
                    // Get the latest commit message
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    env.DOCKER_IMAGE_NAME = commitMessage
                    // Print out for debugging
                    echo "Docker Image Name: ${env.DOCKER_IMAGE_NAME}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.DOCKER_IMAGE_NAME) {
                        sh """
                        docker build -t ${env.DOCKER_IMAGE_NAME} .
                        """
                    } else {
                        error "DOCKER_IMAGE_NAME is not set. Please ensure the commit message contains the image name."
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
