pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('1') // Replace with your credentials ID
        IMAGE_NAME = "mehdifk/devops_tp2"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/FkihMehdi/sprint_boot_devops_tp2.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:0")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', '1') {
                        docker.image("${IMAGE_NAME}:0").push()
                    }
                }
            }
        }
    }
}
