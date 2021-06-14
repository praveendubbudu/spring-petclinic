pipeline {
  agent any
    
  stages {
    stage('build') {
      steps {
        script {
        maven 'Maven3'
        sh 'mvn clean install'
      }
    }
   }
    stage ('Code Quality scan')  {
      steps {
       withSonarQubeEnv('SonarQube') {
       sh "mvn sonar:sonar"
        }
    } 
    }   
    stage ('Archieve artifacts') {
      steps {
              archiveArtifacts artifacts: 'target/*.war, target/*.jar', onlyIfSuccessful: true
      }
    }              
    stage ('deploy') {
      steps {
        deploy adapters: [tomcat8(credentialsId: 'Tomcat_logins', path: '', url: 'http://143.110.176.126:8080/')], contextPath: 'petclinic', war: '**/*.war'
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
