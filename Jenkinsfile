pipeline {
    agent any 
    tools { 
        maven 'Maven-3' 
      
    }
stages { 
     
 stage('Preparation') { 
     steps {
// for display purposes

      // Get some code from a GitHub repository

      git 'https://github.com/akhil-45/hello-world-servlet.git'

      // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     }
   }

   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
  stage('Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.war'
      }
 }
 stage('Sonarqube') {
    environment {
        scannerHome = tool 'sonarqube'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
  //      timeout(time: 10, unit: 'MINUTES') {
 //           waitForQualityGate abortPipeline: true
  //      }
    }
}
     stage('Artifact upload') {
      steps {
     nexusArtifactUploader credentialsId: 'nexus-cred', groupId: 'com.geekcap.vmturbo', nexusUrl: '13.232.34.253:8081/nexus/', nexusVersion: 'nexus2', protocol: 'http', repository: 'release', version: '$BUILD_NUMBER'      }
 }
}
post {
        success {
            mail to:"akhilmuniganti45@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build success"
        }
        failure {
            mail to:"akhilmuniganti45@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        }
    }       
}
