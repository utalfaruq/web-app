pipeline{
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/utalfaruq/web-app.git'
            }
        }

        stage('maven build'){
            steps{
                sh 'mvn clean package'
            }
        }

        stage('code analysis'){
            environment{
               ScannerHome = tool 'sonar' 
            }
            steps{
               script{
                   withSonarQubeEnv('sonar'){
                       sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=alfaruq-webapp"
                   }
               } 
            }
        }

        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
          }
      }

stage('nexus uploads'){
            steps{
               nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/jomacs-webapp-jenkinsfile/target/web-app.war', type: 'war']], credentialsId: 'nexus-credentials', groupId: 'com.mt', nexusUrl: '44.202.2.109:8081/repo/jomacs-webapp', nexusVersion: 'nexus3', protocol: 'http', repository: 'jomacs-webapp', version: '3.8.1-snapshot'
            }
        }

    }
}