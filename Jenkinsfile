node{

def mavenHome = tool name: 'maven3.9.2'

echo "Job name is: $JOB_NAME"
echo "node name is: $NODE_NAME"
echo "Jenkins url is: $JENKINS_HOME"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])


stage('checkout code from GIT'){

git branch: 'Development', credentialsId: 'e5cf3923-9e46-4c2b-af06-ca8973ce4974', url: 'https://github.com/iamlavanthota/devops.git'

}

stage('Build'){
    
sh "${mavenHome}/bin/mvn clean package"

}

stage('ExcuteSonarQubeReport'){

sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactintoNexus'){

sh "${mavenHome}/bin/mvn clean deploy"

    
}
stage('Deploy app into Tomcat server'){

sshagent(['fd04645b-bd6c-492b-b8c6-f7cf915d3de6']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.142.98.26:/opt/apache-tomcat-9.0.91/webapps/"
}
}
}
