def remote = [name: 'tomcat1', host: '206.189.141.153', user: 'listany', password : '123456', allowAnyHosts: true]
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
    stage ('Artifacts upload to nexus') {
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'nexus_logins', groupId: 'org.springframework.samples', nexusUrl: '10.122.0.2:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'listany-admin-snapshots', version: '4.2.5-SNAPSHOT'
		}
    }
	stage ('SSH steps') {
	  steps {
		sshcommand remote: remote, command 'cd /opt/apache-tomcat-7.0.109'
		sshcommand remote: remote, command './bin/shutdown.sh'
		}
	  }
	stage ('deploy') {
	steps {
			deploy adapters: [tomcat8(credentialsId: 'deploy_user1', path: '', url: 'http://206.189.141.153:8080/')], contextPath: 'petclinic', war: '**/*.war'
        }
       }
      }
     }    
