node {
     agent {
        docker {
            image 'maven:3.8.5-openjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }


    stage('1-Clone the code') {
       git credentialsId: 'GitHubcredentials', url: 'https://github.com/mnforba/maven-web-application' 
    }
    stage('2-mavenBuild') {
      sh "${MavenHome}/bin/mvn clean package"
    }
    stage('3-CodeQualityReview') {
       sh "${MavenHome}/bin/mvn sonar:sonar"     
    }
    stage('4-UploadArtifacts') {
       sh "${MavenHome}/bin/mvn deploy"
    }
   # stage ('5-Deploy-UAT') {
    #   deploy adapters: [tomcat9(credentialsId: 'tomcatcredentials', path: '', url: 'http://54.175.4.27:7000/')], contextPath: null, war: 'target/*.war'
    }
    stage('6-EmailNotification') {
       emailext body: '''Hello Everyone,

       Build from Ebay pipeline project.

       Landmark Technologies''', subject: 'Build status', to: 'developers'
    }
    stage('approval') {
       timeout(time:8, unit: 'HOURS' ) {
        input message: 'Please verify and approve'
       }

    }
    stage('prod-Deploy') {
       deploy adapters: [tomcat9(credentialsId: 'tomcatcredentials', path: '', url: 'http://54.175.4.27:7000/')], contextPath: null, war: 'target/*.war'
    }
  }
