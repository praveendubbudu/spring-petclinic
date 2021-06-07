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
    stage ('deploy') {
      steps {
                deploy adapters: [tomcat7(credentialsId: 'Tomcat_logins', path: '', url: 'http://143.110.176.126:8080/')], contextPath: 'petclinic', war: '**/*.war'
      }
    }
    stage ('Tomcat Restart') {
      steps {
        script {
              def remote = [:]
              remote.name = 'tomcat'
              remote.allowAnyHosts = true
              withCredentials([string(credentialsId: 'tomcat_hostname', variable: 'Hostname'), string(credentialsId: 'ssh_user', variable: 'username'), string(credentialsId: 'listany_password', variable: 'Password')]) {
              remote.host = Hostname
              remote.user = username
              remote.password = Password
                stage('Remote SSH') {
                            sshCommand remote: remote, command: "ls -al"
                            sshCommand remote: remote, command: "sudo service tomcat restart"
        }
       }
      }
     }
    }  
  }
}
