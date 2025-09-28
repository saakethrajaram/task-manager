pipeline {
    agent any

    tools {
        nodejs "node" // must match your NodeJS installation in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('SONARQUBE_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Test') {
            steps {
                sh 'npm install'
                sh 'chmod +x ./node_modules/.bin/jest || true'
                sh 'npx --no-install jest --coverage --reporters=default --reporters=jest-junit || true'
                sh 'mkdir -p test-reports && [ -f junit.xml ] && mv junit.xml test-reports/'
                junit 'test-reports/junit.xml'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN"
                    }
                }
            }
        }
    }
}
