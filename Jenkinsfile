node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

  echo "Job Name : ${env.JOB_NAME}"
  echo "node : ${env.NODE_NAME}"
  echo "no : ${env.BUILD_NUMBER}"
  

def mavenHome = tool name: "maven3.8.4"
stage('CheckoutCode')
{
git branch: 'development', credentialsId: 'e5262c9e-56f9-4358-8a54-61cb5b23a537', url: 'https://github.com/sudeesudi/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
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
}
