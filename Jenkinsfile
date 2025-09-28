pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'echo Build step passed!'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo All tests passed!'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying project...'
                sh 'echo Deployment step passed!'
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished successfully!'
        }
    }
}
