pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "sahnounhoussem0501/devops_TP2"
    }

    tools {
        maven 'Maven'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Application') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:0 ."
                    sh "docker tag ${DOCKER_IMAGE}:0 ${DOCKER_IMAGE}:latest"
                }
            }
        } 
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    passwordVariable: 'DOCKER_PASSWORD',
                    usernameVariable: 'DOCKER_USERNAME'
                )]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    sh "docker push ${DOCKER_IMAGE}:0"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                    sh "docker logout"
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                cleanWs()
                sh "docker rmi ${DOCKER_IMAGE}:0 || true"
                sh "docker rmi ${DOCKER_IMAGE}:latest || true"
            }
        }
    }
}