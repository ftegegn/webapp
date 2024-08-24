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
            sh 'sshpass -p "bits" scp -o StrictHostKeyChecking=no target/*.war bits@10.1.0.23:/home/bits/prod/apache-tomcat-9.0.93/webapps/webapp.war' 
        } 
      }
    }
  }
}
