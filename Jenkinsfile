pipeline {
  agent any
  stages {      
    stage('Build && SonarQube analysis') {
	tools {
		maven 'Maven3'
		jdk 'JDK8'
	}
	steps {
		withSonarQubeEnv('SonarQube') {
		//Steps to build and scan the code.
			sh 'mvn clean install sonar:sonar -X -e'	
		}
	 }
  } 
 }
}
