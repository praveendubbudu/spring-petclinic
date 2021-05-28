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
          nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'Nexus_Credentials', groupId: 'org.springframework.samples', nexusUrl: '10.122.0.2:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'listany-admin-snapshots', version: '4.2.5-SNAPSHOT'
      }
    }
    stage ('deploy') {
      steps {
        sshagent(['deploy_user']) {
            sh 'ssh -o StrictHostKeyChecking=no root@165.232.185.75
            sh 'cd /opt/apache-tomcat-7.0.109'
            sh './bin/shutdown.sh'
            sh 'scp -o StrictHostKeyChecking=no petclinic/target/petclinic.war root@165.232.185.75:/opt/apache-tomcat-7.0.109/webapps/pet'
            sh 'cd /opt/apache-tomcat-7.0.109'
            sh './bin/startup.sh'
       }
     }
   }
 }
}
