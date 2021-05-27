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
        nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/*.war', type: 'war']], credentialsId: 'Nexus_Credentials', groupId: 'org.springframework.samples', nexusUrl: '134.209.159.124:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://134.209.159.124:8081/repository/listany-admin-b2c/', version: '4.2.5-SNAPSHOT'
      }
  }
}
