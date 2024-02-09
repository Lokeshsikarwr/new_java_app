pipeline {
	agent {
		label "ubuntu-slave"
	}
	stages {
		stage ("Pulling the code from SCM") {
			steps {
				git branch: 'main', url: 'https://github.com/gouravaas/new_java_app.git'
			}
		}
		stage ("Build the code") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
                                sh 'sudo mvn clean package'
			}
		}
		stage ("Create docker image"){
			steps {
				sh 'sudo docker build -t java-app:$BUILD_TAG .'
				sh 'sudo docker tag java-app:$BUILD_TAG gouravaas/java-app:$BUILD_TAG'
			}
		}
		stage ("Push on Docker-Hub"){
			steps {
				withCredentials([string(credentialsId: 'Docker_pass_ID', variable: 'docker_hub_pass_var')]) {
					sh 'sudo docker login -u gouravaas -p ${docker_hub_pass_var}'
					sh 'sudo docker push gouravaas/java-app:$BUILD_TAG' 		
				}
			}
		}
		stage ("Testing the Build"){
			steps {
				sh 'sudo docker rm -f $(docker ps -a -q)'
				sh 'sudo docker run -dit --name java-test$BUILD_TAG -p 8080:8080 gouravaas/java-app:$BUILD_TAG'
			}
		}
		stage ("QAT Testing"){
			steps {
				sh 'sudo curl --silent http://13.235.128.80:8080/java-web-app/ | grep -i "india"'
			}
		}
		stage ("Approval from QAT"){
			steps {
				script {
					Boolean userInput = input(id: 'Proceed1', message: 'Do you want to Promote this build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                				echo 'userInput: ' + userInput
				}
			}
		}
		stage ("Prod ENV"){
			steps{
				sshagent(credentials:['sshagent-cred']) {
			    	 	sh "ssh -o StrictHostKeyChecking=no ubuntu@13.215.193.66 sudo docker run  -dit  -p  :8080  gouravaas/java-app:$BUILD_TAG"
				}
			}
		}
	}
}
