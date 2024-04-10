pipeline {
        agent {
                label "ubuntu-slave"
        }
        stages {
                stage ("Pulling the code from SCM") {
                        steps {
                                git branch: 'main', url: 'https://github.com/Lokeshsikarwr/new_java_app.git'
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
				sh 'sudo docker run -dit --name java-app -p 8080:8080 java-app:$BUILD_TAG'
                        }
                }
	}
}

