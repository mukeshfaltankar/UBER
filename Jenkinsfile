// Define the URL of the Artifactory registry
def registry = 'https://trialpihnaz.jfrog.io/'
pipeline {
 agent any
 environment {
               PATH="/opt/maven/bin:$PATH"
             }
 stages {
         stage('build') {
           steps { 
                  echo "---build started---"
                  sh 'mvn clean deploy -Dmaven.test.skip=true'
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
         stage("Jar Publish") { 
           steps { 
             script {
              echo ' < --- jar publish started --- > '
              def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "artifact-cred"
              def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
              def uploadSpec = """{ 
                "files":[
                { 
                "pattern": "jarstaging/(*)",
                "target": "sai-libs-release-local/{1}",
                "flat": "false",
                "props": "${properties}",
                "exclusions": [ "*.sha1", "*.md5"]
                }
                ]
                }"""
             def buildInfo = server.upload(uploadSpec)
             buildInfo.env.collect()
             server.publishBuildInfo(buildInfo)

              echo ' < --- jar publish ended --- > '
             }
           }
         }
              }
        }


 
