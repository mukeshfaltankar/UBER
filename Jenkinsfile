pipeline {
 agent any
 environment {
               PATH="/opt/maven/bin:$PATH"
             }
 stages {
         stage('build') {
           steps { 
                  echo "---build started---"
                  sh 'mvn clean package -Dmaven.test.skip=true'
                  echo "---build completely---"
                 }
               }
         stage("test") {
           steps { 
                 echo "---unit test started---"
                 sh 'mvn surefire-report:report'
                 echo " ---unit test completed ---"
                  }
               }
         stage('sonarqube analysis') {
             environment { 
              scannerHome = tool 'mukesh-sonar-scanner'
                         }
           steps {
             withSonarQubeEnv('mukesh-sonarqube-server') {
              sh "${scannerHome}/bin/sonar-scanner"
              } 
                }
               }
              }
        }


 
