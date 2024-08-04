pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = sh(returnStdout: true, script: "git log -1 --pretty=%B").trim()
    }
    stages {
        stage('Checkokut') {
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
        
                docker build -t ${env.DOCKER_IMAGE_NAME} .
                docker login nisha-ThinkPad-T470 -u nisha -p 1234
               
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
