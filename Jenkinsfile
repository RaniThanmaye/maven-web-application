node {

def MavenHome = tool name : 'Maven3.9.10'

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')),
[$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], pipelineTriggers([pollSCM('* * * * *')])])

echo "build number ${env.BUILD_NUMBER}"
echo " jenkins home directory ${env.JENKINS_HOME}"


stage('CheckoutCode'){
git branch: 'development', credentialsId: '7798f564-01c9-48f6-b0e0-585f1afc0221', url: 'https://github.com/RaniThanmaye/maven-web-application.git'
}

stage('Build'){
sh "${MavenHome}/bin/mvn clean package"
}

stage('Executesonarreoprt'){
sh "${MavenHome}/bin/mvn clean package sonar:sonar"
}

stage('UploadartifactToNexus'){
sh "${MavenHome}/bin/mvn clean package deploy"
}
stage('deployToTomcat'){
sshagent(['c44ea89a-a817-4eef-b6a2-7313b2c6f22b']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.94.101:/opt/apache-tomcat-9.0.106/webapps/"
}
}
}
