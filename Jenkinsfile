node{
  
    def mavenHome = tool name: "Maven_3.8.6"
    
    //echo "Branch name is: ${env.BRANCH_NAME}"
    echo "Build number: ${env.BUILD_NUMBER}"
    echo "Job name is: ${env.JOB_NAME}"
    echo "Node name is: ${env.NODE_NAME}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
  try{
    sendslacknotifications("STARTED")
stage('CheckoutCode'){
git branch: 'development', credentialsId: '3667ec9e-7827-40b5-bf42-acadbaca2263', url: 'https://github.com/Leelakrishna-Devops/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean packag"
}
  /*
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployAppIntoTomcatServer'){
sshagent(['']){
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.18.55:/opt/apache-tomcat-9.0.68/webapps/"
}
}
*/
 }//try block close
  catch(e){
currentBuild.result = "FAILURE"
}//catch block close
  finally{
sendslacknotifications(currentBuild.result)
}//finally close
}//Node Closing
def sendslacknotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
