pipeline{

    agent any


    tools {
        maven 'Maven'
    }

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
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
                    sh "docker build -t devops_tp2:0 ."
                    sh "docker tag devops_tp2:0 devops_tp2:latest"
                }
            }
        } 

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push devops_tp2:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}