pipeline {
	agent any
	environment {
		IMAGE_NAME = 'simple-todo:latest'
	}

	stage {
		stage('checkout') {
			steps {
				git branch: 'main', url: 'https://github.com/cygday/simple-todo-app.git'
			}
		}

		stage('sonarqube scan') {
			steps {
				withSonarQubeEnv('MySonarQube'){
					sh 'sonar-scanner'
					}
				}
			}
		}
			
		stage('Docker build') {
			steps {
				sh 'docker build -t $IMAGE_NAME:latest . '
			}
		}
		stage('run container') {
			steps {
				sh 'docker run -d -p 8090:80 --name todo-nginx $IMAGE_NAME:latest || true'
			}
		}
	}
}
