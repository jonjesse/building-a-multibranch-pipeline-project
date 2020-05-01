pipeline {
    agent {
        docker {
            image 'node:14-alpine'
            args '-p 3000:3000 -p 5000:5000' 
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'dev' 
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'DEV::Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'prod'  
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'PROD::Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
   }
}
