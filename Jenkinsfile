pipeline{
    agent any

    toos {
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
    }
}