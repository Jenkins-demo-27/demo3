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


        stage('Build Docker mmImage') {
            steps {
                script {
                    sh "docker build -t ${env.DOCKER_IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
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
