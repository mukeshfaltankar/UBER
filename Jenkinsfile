pipeline {
 agent any
 environment {
               PATH="/opt/maven/bin:$PATH"
             }
 stages {
         stage('build') {
           steps { 
                  sh 'mvn clean deploy'
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


 
