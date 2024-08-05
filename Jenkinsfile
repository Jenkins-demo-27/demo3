pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = sh(returnStdout: true, script: "git log -1 --pretty=%B").trim()
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

        stage('Build Docker Image') {
            steps {
                sh """

                docker login 127.0.0.1:443 -u nisha -p 1234

                cd demo3  # Ensure you are in the correct directory
                docker build -t ${env.DOCKER_IMAGE_NAME} .
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                docker run -idt -p 8000:8000 --name ${env.DOCKER_IMAGE_NAME}-container ${env.DOCKER_IMAGE_NAME}
                """
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
