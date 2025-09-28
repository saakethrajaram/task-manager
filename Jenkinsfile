pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // your Sonar token from Jenkins credentials
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

                echo 'Running tests...'
                sh 'npx jest --coverage --reporters=default --reporters=jest-junit'

                sh 'mkdir -p test-reports'
                sh 'mv junit.xml test-reports/ || true'
                junit 'test-reports/junit.xml'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                sh "sonar-scanner -Dsonar.login=$SONAR_TOKEN"
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
            echo 'Pipeline failed!'
        }
    }
}
