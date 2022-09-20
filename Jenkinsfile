def version = "${env.BUILD_ID}"
pipeline{

agent any

tools{
maven 'maven3.8.2'

}

// triggers{
// pollSCM('* * * * *')
// }

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'master', credentialsId: '957b543e-6f77-4cef-9aec-82e9b0230975', url: 'https://github.com/wipro-pvt-limited/maven-web-application.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }
	
  stage('Docker image build'){
  steps{
  sh  """\
  #!/bin/bash -l
  docker build -t mithunwebappkareem:${version} .
  """
  }
  }
  stage('Docker login'){
  steps{
  sh  """\
  #!/bin/bash -l
  docker login -u shaikfayazz444 -p 8688949515
  docker tag mithunwebappkareem:${version} shaikfayazz444/kareem:${version}
  """
  }
  }
  stage('Docker push'){
  steps{
  sh  """\
  #!/bin/bash -l
  docker push shaikfayazz444/kareem:${version}
  """
  }
  }
/*
 stage('ExecuteSonarQubeReport'){
  steps{
  sh  "mvn clean sonar:sonar"
  }
  }
  
  stage('UploadArtifactsIntoNexus'){
  steps{
  sh  "mvn clean deploy"
  }
  }
  
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

 success{
 emailext to: 'shaiakfayazz444@gmail.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'shaikfayazz444@gmail.com'
 }
 
 failure{
 emailext to: 'shaikfayazz444@gmail.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'shaikfayazz444@gmail.com'
 }
 
}


}//Pipeline closing
