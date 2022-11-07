pipeline{

agent any

tools{
maven '3.8.4'

}

triggers{
pollSCM('* * * * *')
}

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
	  git credentialsId: 'Github-naga-user', url: 'https://github.com/MittaNagaraju/maven-web-application.git'
	}
  }
	
  stage('Build'){
    steps{
      sh  "mvn clean package"
    }
  }
  
  stage('ExecuteSonarQubeReport'){
    steps{
      withSonarQubeEnv('sonarqube-8.9.10') {
        sh  "mvn sonar:sonar"
      }
    }
  }
  stage('UploadArtifactsIntoNexus'){
    steps{
       /*nexusPublisher nexusInstanceId: 'nexus3', nexusRepositoryId: 'sample', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/home/jenkins/jenkins-workspace/workspace/JavaMavenProj/target/maven-web-application.war']], mavenCoordinate: [artifactId: 'JavaMavenProj', groupId: 'org.jenkins-ci.main', packaging: 'war', version: '1.0.1']]]*/
       nexusPublisher nexusInstanceId: 'nexus3', nexusRepositoryId: 'sample', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/home/jenkins/jenkins-workspace/workspace/JavaMavenProj/target/maven-web-application.war']], mavenCoordinate: [artifactId: 'Maven_project', groupId: 'com.jenkins-ci.main', packaging: 'war', version: '1.0.1']]]
    }
  }
 /* 
  stage('DeployAppIntoTomcat'){
  steps{
  sshagent(['bfe1b3c1-c29b-4a4d-b97a-c068b7748cd0']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.190.162:/opt/apache-tomcat-9.0.50/webapps/"    
  }
  }
  }
  */
}//Stages Closing

post{
 always{
         emailext body: 'check the build status ', subject: 'pipeline status ', to: ''
        }
 }


}//Pipeline closing
