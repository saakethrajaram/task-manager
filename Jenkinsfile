pipeline {
    agent any
    tools { nodejs 'NodeJS_24' } // NodeJS configured in Jenkins
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Install & Test') {
            steps {
                sh 'npm install'
                sh './node_modules/.bin/jest --coverage --reporters=default --reporters=jest-junit'
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
    post { always { echo 'Pipeline finished.' } }
}
