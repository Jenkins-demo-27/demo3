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
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()

                    // Extract image name and tag from the commit message
                    def matcher = (commitMessage =~ /imageName:(\S+)\s*version:(\S+)/)
                    if (matcher) {
                        env.DOCKER_IMAGE_NAME = matcher[0][1]
                        env.DOCKER_IMAGE_TAG = matcher[0][2]
                    } else {
                        error "Commit message does not contain the required 'imageName' and 'version' format."
                    }
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
                    sh "docker run -d --name ${env.DOCKER_IMAGE_NAME}-container ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}"
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
