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


    }
}