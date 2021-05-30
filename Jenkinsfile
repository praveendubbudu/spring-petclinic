def remote = [:]
remote.name = "tomcat"
remote.host = "165.232.185.75"
remote.allowAnyHosts = true

node {
	agent any
			tools {
				maven 'Maven3'
			}
		stages {
			stage ('check out') {
				steps {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_logins', url: 'https://github.com/praveendubbudu/spring-petclinic.git']]])
				}
			}
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
        stage("Deploy") {
			  withCredentials([sshUserPrivateKey(credentialsId: 'ssh-logins', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'root')]) {
				  remote.user = root
				  remote.identityFile = identity
			
				  sshCommand remote: remote, command: 'su listany', failOnError:'false'
				  sshCommand remote: remote, command: 'cd /opt/apache-tomcat-7.0.109'
				  sshCommand remote: remote, command: './bin/shutdown.sh'
				  sshPut remote: remote, from: '/var/lib/jenkins/workspace/petclinic/target/petclinic.war', into: '/opt/apache-tomcat-7.0.109/webapps'
				  sshCommand remote: remote, command: 'cd /opt/apache-tomcat-7.0.109'
				  sshCommand remote: remote, command: './bin/startup.sh'	
				}
        
			}
		}
	
	}
