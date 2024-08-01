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
                              def commitMessage = sh(script: 'git log -1 --pretty=%B')
                              env.DOCKER_IMAGE_NAME = commitMessage
                    // Extract image name and tag from the commit message

                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.DOCKER_IMAGE_NAME) {
                        sh """
                        docker build -t ${DOCKER_IMAGE_NAME} .
                        """
                    } else {
                        error "DOCKER_IMAGE_NAME is not set. Please ensure the commit message contains the image name."
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
