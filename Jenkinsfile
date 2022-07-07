node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

  echo "Job Name : ${env.JOB_NAME}"
  echo "node : ${env.NODE_NAME}"
  echo "no : ${env.BUILD_NUMBER}"
  
def mavenHome = tool name: "maven3.8.4"
try {
  stage('CheckoutCode')
{
git branch: 'development', credentialsId: 'e5262c9e-56f9-4358-8a54-61cb5b23a537', url: 'https://github.com/sudeesudi/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
/*
stage('ExecutrSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer')
{
sshagent(['3de98e2d-513c-4b3e-8596-a7772389f3ee']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.32.129:/opt/apache-tomcat-9.0.64/webapps"
}
}
*/
} 
	catch (e)
	{
        currentBuild.result = "FAILED"
        throw e
    }
	finally {
        sendSlackNotifications(curentBuild.result)
    }
}

def sendSlackNotifications(String buildStatus = 'STARTED') 
{
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
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }
}
