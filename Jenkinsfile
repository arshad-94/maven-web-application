node
{
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '10', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def maven= tool name:"maven 3.8.5"
stage('get code from git'){
git branch: 'development', credentialsId: '22d2e961-337c-4f32-8025-af6fa97cea50', url: 'https://github.com/arshad-94/maven-web-application.git'
}    
stage('build')
{
sh "${maven}/bin/mvn clean package"
 }
/*
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
   sh "scp -o   StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.147.23:/opt/apache-tomcat-9.0.65/webapps/"
}
}
*/
}
