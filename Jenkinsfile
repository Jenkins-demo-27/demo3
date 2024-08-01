pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
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
