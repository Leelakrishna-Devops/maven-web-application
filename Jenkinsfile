node{
  
    def mavenHome = tool name: "Maven_3.8.6"
    
    //echo "Branch name is: ${env.BRANCH_NAME}"
    echo "Build number: ${env.BUILD_NUMBER}"
    echo "Job name is: ${env.JOB_NAME}"
    echo "Node name is: ${env.NODE_NAME}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
stage('CheckoutCode'){
git branch: 'development', credentialsId: '3667ec9e-7827-40b5-bf42-acadbaca2263', url: 'https://github.com/Leelakrishna-Devops/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
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
}//Node Closing
