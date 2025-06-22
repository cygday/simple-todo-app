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
		
			
		stage('Docker build') {
			steps {
				sh 'docker build -t $IMAGE_NAME . '
			}
		}
		stage('push to dockerhub') {
			steps {
				sh ' pushing image to docker hub'
				withCredentials([usernamePassword(
					credentialsId:'to-do-app',
					passwordVariable:'dockerhubpass',
					usernameVariable:'dockerhubuser'
					)]){
				sh 'docker login -u ${env.dockerhubuser} -p ${dockerhubpass}'
				sh ' docker tag  simple-todo-app:latest ${env.dockerhubuser}/simple-todo-app:latest'
				sh 'docker push ${env.dockerhubuser}/simple-todo-app:latest'
				echo 'docker push success'
				}
			}
		}
		stage('docker compose up') {
			steps {
				sh 'docker compose up --build -d'
			}
		}
	}
}
