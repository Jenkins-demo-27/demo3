pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = ''
    }

    stages {
        stage('Checkout') {
            steps {
                sh """
                git clone "https://github.com/Jenkins-demo-27/demo3.git"
                cd demo3
                """
            }
        }

        stage('Extract Image Info') {
            steps {
                script {
                    // Get the ldast ceommit message
                    env.DOCKER_IMAGE_NAME = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    // Print out for debugging
                    echo "Docker Image Name: ${env.DOCKER_IMAGE_NAME}"
                    // Extract image name and tag from the commit message

                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${env.DOCKER_IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d --name ${env.DOCKER_IMAGE_NAME}-container ${env.DOCKER_IMAGE_NAME}"
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
