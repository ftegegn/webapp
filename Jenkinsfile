pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }

    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage ('Deploy-to-Tomcat') {
      steps {
         sshagent(['tomcat']) {
            //sh 'scp -o StrictHostKeyChecking=no target/*.war bits@10.1.0.23:/prod/apache-tomcat-9.0.93/webapps/webapp.war'
             sh 'ssh bits@196.188.53.87'
        } 
      }
    }
  }
}
