pipeline {
    agent any

    tools {
        nodejs "node" // must match your NodeJS installation in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('SONARQUBE_TOKEN')  // SonarQube secret text credential ID
        SNYK_TOKEN = credentials('SNYK_TOKEN')        // Snyk secret text credential ID
    }

    stages {

        // 1. Build Stage
        stage('Build') {
            steps {
                echo "Building Node.js application..."
                sh 'node -v'
                sh 'npm -v'
                sh 'npm install'
                sh 'npm run build || echo "No build step, skipping..."'
            }
        }

        // 2. Test Stage
        stage('Test') {
            steps {
                echo "Running unit tests..."
                sh 'npm test -- --ci --reporters=default --reporters=jest-junit'
            }
            post {
                always {
                    junit 'junit.xml'
                }
            }
        }

        // 3. Code Quality Stage
        stage('Code Quality') {
            steps {
                echo "Running SonarQube analysis..."
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=task-manager -Dsonar.sources=.'
                }
            }
        }

        // 4. Security Stage
        stage('Security Scan') {
            steps {
                echo "Running Snyk security scan..."
                sh 'snyk auth $SNYK_TOKEN'
                sh 'snyk test || true'
            }
        }

        // 5. Deploy Stage (Simulated)
        stage('Deploy to Staging') {
            steps {
                echo "Simulating deployment to staging environment..."
                sh 'echo "App deployed to staging (simulation)"'
            }
        }

        // 6. Release Stage (Simulated)
        stage('Release to Production') {
            when {
                branch 'main'
            }
            steps {
                echo "Simulating release to production..."
                sh 'echo "App released to production (simulation)"'
            }
        }

        // 7. Monitoring Stage
        stage('Monitoring & Alerts') {
            steps {
                echo "Monitoring application..."
                sh 'echo "App health check simulated"'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
