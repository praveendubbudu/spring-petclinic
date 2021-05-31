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
              archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
      }
    }              
    stage ('Artifacts upload to nexus') {
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'nexus_logins', groupId: 'org.springframework.samples', nexusUrl: '10.122.0.2:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'listany-admin-snapshots', version: '4.2.5-SNAPSHOT'
		}
    }
	stage ('deploy') {
	steps {
			deploy adapters: [tomcat8(credentialsId: 'deploy_user1', path: '', url: 'http://206.189.141.153:8080/')], contextPath: 'petclinic', war: '**/*.war'
        }
       }
      }
     }    
