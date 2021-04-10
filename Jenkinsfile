node  
{
//echo "GitHub BranhName ${env.BRANCH_NAME}"
  //echo "Jenkins Job Number ${env.BUILD_NUMBER}"
    echo "Jenkins Node Name ${env.NODE_NAME}"
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '5'))])      
def mavenHome = tool name: "maven3.6.3"                            

  stage('checkout code') 
{

 git credentialsId: '15437951-7739-45bf-8d9f-7327e125c0d2', url: 'https://github.com/vanathi-devops/maven-web-application.git'
}
stage('build')
{
sh "${mavenHome}/bin/mvn clean package"  
}

 stage('ExecuteSonarqubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"  
}

stage('UploadArtifactintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"  
}
stage('DeployAppintoTomcatserver')
{ 
sshagent(['b019308c-e7b0-401f-bd82-608606bdb13c']){
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.35.144:/opt/tomcat9/webapps/"
}
}
stage('sendemailnotification')
{
emailext body: '''BuildOver -scriptedway
Regards,
Vanathi''', subject: 'Build Over - scriptedway', to: 'vanathipandithan.vg@gmail.com'
}
}
