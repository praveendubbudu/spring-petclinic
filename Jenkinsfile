pipeline {
  agent any
  tools {
    maven 'Maven3'
      }
  stages {
    stage('build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage ('deploy') {
      steps {
        deploy adapters: [tomcat7(credentialsId: 'tomcat_logins', path: '', url: 'http://165.232.185.75:8080/')], contextPath: 'petclinic', war: '**/*.war'
       }
      }
    }
   }

