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
		stage ("Testing the Build"){
			steps {
				sh 'sudo docker build -t java-app:$BUILD_TAG .'
				sh 'sudo docker tag java-app:$BUILD_TAG gouravaas/java-app:$BUILD_TAG'
			}
		}
		stage ("Push on Docker-Hub"){
			steps {
				withCredentials([string(credentialsId: 'Docker_pass_ID', variable: 'docker_hub_pass_var')]) {
					sh 'sudo docker login -u gouravaas -p ${docker_hub_pass_var}
					sh 'sudo docker push gouravaas/java-app:$BUILD_TAG 
    					
				}
			}
		}
	}

}
