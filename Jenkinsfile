properties([disableConcurrentBuilds()])
pipeline {
    options {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
        timestamps()
    }
    agent {
        docker {
            image 'node:21.2.0-alpine3.17'
            args '-p 3000:3000 -p 5000:5000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                
                //sh 'npm install create-vite@latest'
                //sh 'npm init vue@latest'
                sh 'npm install create-vite@5.0.0'
                //sh 'npm create vite@latest my-vue-app -- --template vue-ts'
                sh 'npm init vite@latest my-vue-app -- --template vue'
                sh 'pwd'
                sh 'cd  /var/jenkins_home/workspace/create-vue_main'
                sh 'pwd ./my-vue-ap'
                sh 'ls -a'
                sh 'npm install'
                sh 'npm run dev'
                //sh 'npm create vue@latest'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development' 
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'  
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
