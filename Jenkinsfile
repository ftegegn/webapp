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
   
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/ftegegn/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }

    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp || true'
         sh 'wget --no-check-certificate --content-disposition https://raw.githubusercontent.com/ftegegn/webapp/master/owasp-dependency-check.sh'
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
      }
    }

    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
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

    stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'sshpass -p "B1tspr6" ssh -o StrictHostKeyChecking=no bitsapsprd@10.1.0.17 "docker run -t zaproxy/zap-stable zap-baseline.py -t http://10.1.0.23:8080/webapp/" || true'
        }
      }
    }
    
  }
}
