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

    stage ('Source Composition Analysis') {
      steps {
         sh 'rm test* || true'
         sh 'wget https://github.com/ftegegn/webapp/blob/master/test.sh'
         sh 'chmod +x test.sh'
         sh 'bash test.sh'
         //sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
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
