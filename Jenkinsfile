pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // Your Jenkins Sonar token
    }

    tools {
        nodejs 'NodeJS_24' // Match the name you gave in Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Test') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'

                script {
                    def packageJson = readJSON file: 'package.json'
                    if (packageJson.scripts?.build) {
                        echo 'Running build...'
                        sh 'npm run build'
                    } else {
                        echo 'No build script found, skipping build.'
                    }
                }

                echo 'Running tests...'
                sh 'chmod +x ./node_modules/.bin/jest'
                sh 'npx --no-install jest --coverage --reporters=default --reporters=jest-junit'
                sh 'mkdir -p test-reports'
                sh 'mv junit.xml test-reports/ || true'
                junit 'test-reports/junit.xml'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
