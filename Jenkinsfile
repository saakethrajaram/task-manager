pipeline {
    agent any
    tools {
        nodejs 'NodeJS_24' // must match the NodeJS name in Jenkins config
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Install & Test') {
            steps {
                sh 'npm install'
                sh 'npx jest --coverage --reporters=default --reporters=jest-junit'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh "sonar-scanner -Dsonar.login=$SONAR_TOKEN"
                }
            }
        }
    }
    post {
        always { echo 'Pipeline finished.' }
    }
}
