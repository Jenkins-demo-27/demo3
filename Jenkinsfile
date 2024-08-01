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
                    // Get the last commit message
                    sh '''
                    DOCKER_IMAGE_NAME=$(git log -1 --pretty=%B)
                    '''
                    // Extract image name and tag from the commit message

                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d --name ${env.DOCKER_IMAGE_NAME}-container ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG} -p 8003:8000"
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
