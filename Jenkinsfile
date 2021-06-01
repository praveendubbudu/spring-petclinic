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
    stage ('Archieve artifacts') {
      steps {
              archiveArtifacts artifacts: 'target/*.war, target/*.jar', onlyIfSuccessful: true
      }
    }              
    stage ('Artifacts upload to nexus') {
      steps {
                nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'Nexus_Credentials', groupId: 'org.springframework.samples', nexusUrl: '134.209.159.124:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'petclinic', version: '4.2.5-SNAPSHOT'
    }
  }
    stage ('deploy') {
      steps {
                deploy adapters: [tomcat7(credentialsId: 'Tomcat_logins', path: '', url: 'http://143.110.176.126:8080/')], contextPath: 'petclinic', war: '**/*.war'
        }
       }
      }
     }
