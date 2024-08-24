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
         sshagent(['1ead76f4-7e8e-4f49-89a5-6ee12c40d7f4']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.war bits@10.1.0.23:/prod/apache-tomcat-9.0.93/webapps/webapp.war'
        }  
      }
    }
  }
}
