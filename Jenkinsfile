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
                       sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=utalfaruq-webapp"
                   }
               } 
            }
        }

stage('nexus uploads'){
            steps{
               nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/jomacs-webapp-jenkinsfile/target/web-app.war', type: 'war']], credentialsId: 'nexus-credentials', groupId: 'com.mt', nexusUrl: '44.202.2.109:8081/repository/jomacs-webapp', nexusVersion: 'nexus3', protocol: 'http', repository: 'jomacs-webapp', version: '3.8.1-snapshot'
            }
        }

stage('prod deployment'){
    steps{
deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://3.90.230.83:8080/')], contextPath: null, war: 'target/web-app.war'
}
}

    }
post {
        success {
            slackSend channel: 'jomacs-practice', color: 'good', message: "Build successful: ${currentBuild.fullDisplayName}"
        }
        failure {
            slackSend channel: 'jomacs-practice', color: 'danger', message: "Build failed: ${currentBuild.fullDisplayName}"
        }
        aborted {
            slackSend channel: 'jomacs-practice', color: 'warning', message: "Build aborted: ${currentBuild.fullDisplayName}"
        }
}


    
}