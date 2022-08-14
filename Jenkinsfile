node
{
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '10', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def maven= tool name:"maven 3.8.5"
try{
	slacknotification('STARTED') 
stage('get code from git'){
git branch: 'development', credentialsId: '22d2e961-337c-4f32-8025-af6fa97cea50', url: 'https://github.com/arshad-94/maven-web-application.git'
}    
stage('build')
{
sh "${maven}/bin/mvn clean package"
}

stage('report')
{
sh "${maven}/bin/mvn sonar:sonar"    
}
stage('uploadartifact')
{
    sh "${maven}/bin/mvn clean deploy"
}
stage('deploy to tomcat')
{
   sshagent(['28cd598e-46ec-44d1-ba15-37dd9f5722cc']){
   sh "scp -o   StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.110.32.204:/opt/apache-tomcat-9.0.65/webapps/"
 }  
}
}

catch (e)
{ // If there was an exception thrown, the build failed
    currentBuild.result = "FAILURE"
    throw e
}
finally {
    // Success or failure, always send notifications
    slacknotification(currentBuild.result)
  }
  }
def slacknotification(String buildStatus = 'STARTED') 
{
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
