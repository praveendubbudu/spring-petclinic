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
    stage ('Tomcat Restart') {
      steps {
        script {
                withCredentials([string(credentialsId: 'Tomcat_HOST_NAME', variable: 'Host'), string(credentialsId: 'Listany_Password', variable: 'Password')]) {
                def remote = [:]
                remote.name = 'tomcat'
                remote.host = '$Host'
                remote.user = 'listany'
                remote.password = '$password'
                remote.allowAnyHosts = true
                sshCommand remote: remote, command: "sudo service tomcat restart"
        }
       }
      }
     }
}
