pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'echo npm install simulated'
            }
        }
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'echo Build step passed!'
            }
        }
        stage('Run Lint') {
            steps {
                echo 'Running lint...'
                sh 'echo Lint step passed!'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'echo All tests passed!'
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging project...'
                sh 'echo Package created!'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying project...'
                sh 'echo Deployment step passed!'
            }
        }
        stage('Clean Up') {
            steps {
                echo 'Cleaning up workspace...'
                sh 'echo Cleanup done!'
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished successfully!'
        }
    }
}
