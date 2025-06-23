pipeline {
	agent any
	environment {
		IMAGE_NAME = 'simple-todo:latest'
	}

	stages {
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
				sh "echo 'pushing image to docker hub'"
				withCredentials([usernamePassword(
					credentialsId:"to-do-app",
					passwordVariable:"dockerhubpass",
					usernameVariable:"dockerhubuser"
					)]){
				sh "docker login -u ${env.dockerhubuser} -p ${dockerhubpass}"
				sh "docker tag  simple-todo-app:latest ${env.dockerhubuser}/simple-todo-app:latest"
				sh "docker push ${env.dockerhubuser}/simple-todo-app:latest"
				echo "docker push success"
				}
			}
		}
		stage('docker compose up') {
			steps {
				sh 'docker compose up --build -d'
			}
		}
	}
	  post {
 
                always {
 
                    mail to: 'amethystpun98@gmail.com',
 
                    subject: "Job '${JOB_NAME}' (${BUILD_NUMBER}) status",
 
                    body: "Please go to ${BUILD_URL} and verify the build"
 
                }

                success {
 
                    mail bcc: '', body: """Hi Team,
 
                    Build #$BUILD_NUMBER is successful, please go through the url
 
                    $BUILD_URL
 
                    and verify the details.
 
                    Regards,
 
                    DevOps Team""", cc: '', from: '', replyTo: '', subject: 'BUILD SUCCESS NOTIFICATION', to: 'amethystpun98@gmail.com'
 
                }

                failure {
 
                        mail bcc: '', body: """Hi Team,
 
                        Build #$BUILD_NUMBER is unsuccessful, please go through the url
 
                        $BUILD_URL
 
                        and verify the details.
 
                        Regards,
 
                        DevOps Team""", cc: '', from: '', replyTo: '', subject: 'BUILD FAILED NOTIFICATION', to: 'amethystpun98@gmail.com'
 
                }
 
         }

}

