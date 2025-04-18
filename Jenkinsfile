pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        IMAGE_NAME = "mehdifk/devops_tp2"
        VERSION = "1.0.0"
        // Add Docker path to ensure it can be found
        PATH = "/usr/local/bin:/usr/bin:/bin:${env.PATH}"
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Install Docker') {
            steps {
                // Check if Docker exists, if not try to install it
                sh '''
                    if ! command -v docker &> /dev/null; then
                        echo "Docker not found. Attempting to install..."
                        curl -fsSL https://get.docker.com -o get-docker.sh
                        sh get-docker.sh
                    else
                        echo "Docker is already installed"
                        docker --version
                    fi
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${VERSION} ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: '1',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:${VERSION}
                    '''
                }
            }
        }
    }
}
