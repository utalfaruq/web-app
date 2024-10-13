pipeline{
    any

    toos {
        maven 'maven'
    }

    stages {
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/utalfaruq/web-app.git'
            }
        }

       
    }
}