pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS', type: 'NodeJS'  // Replace 'NodeJS' with your Jenkins NodeJS tool name
        SONAR_TOKEN = credentials('sonar-token')           // Replace 'sonar-token' with your Jenkins credential ID
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    env.PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
                }
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'chmod +x ./node_modules/.bin/jest || true'
                sh 'npx --no-install jest --coverage --reporters=default --reporters=jest-junit'
                sh 'mkdir -p test-reports && [ -f junit.xml ] && mv junit.xml test-reports/ || echo "No junit.xml generated"'
                junit 'test-reports/*.xml'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with your Jenkins SonarQube server name
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=task-manager -Dsonar.sources=. -Dsonar.host.url=http://10.0.0.3:9000 -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
