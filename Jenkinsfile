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
    stage ('deploy') {
      steps {
        script {
        sshagent(['ssh-logins']) {
            sh 'ssh -t -t listany@165.232.185.75 -o StrictHostKeyChecking=no'
            sh "echo 123456"
            sh 'cd /opt/apache-tomcat-7.0.109'
            sh './bin/shutdown.sh'
            sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/petclinic/target/petclinic.war /opt/apache-tomcat-7.0.109/webapps'
            sh 'cd /opt/apache-tomcat-7.0.109'
            sh './bin/startup.sh'
        }
       }
     }
   }
 }
}
