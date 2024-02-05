pipeline {
	agent {
		label "Jenkins-Slave"
	}
	stages {
		stage ("Pulling the code from SCM") {
			steps {
				git branch: 'main', credentialsId: 'Jenkins-Slave-ID', url: 'https://github.com/gouravaas/new_java_app.git'
			}
		}
		stage ("Installing git") {
			steps {
				sh 'sudo yum install -y git'
			}
		}
		stage ("Build the code") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'

			}
		}
	}

}
