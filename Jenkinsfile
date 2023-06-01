pipeline {
  agent any
  stages {      
    stage('Build && SonarQube analysis') {
	tools {
		maven 'Maven3'
		jdk 'JDK8'
	}
	steps {
		sh 'mvn clean package'	
		}
  } 
 }
}
