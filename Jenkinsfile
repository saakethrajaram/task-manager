pipeline {
    agent any

    tools {
        nodejs "node" // must match your NodeJS installation in Jenkins
    }

    environment {
        REGISTRY = "saakethrajaram/task-manager"
        APP_NAME = "task-manager"
        IMAGE_TAG = "v${env.BUILD_NUMBER}"
        SONAR_TOKEN = credentials('SONARQUBE_TOKEN')  // your SonarQube secret text credential ID
        SNYK_TOKEN = credentials('SNYK_TOKEN')        // your Snyk secret text credential ID
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
                sh "docker build -t $REGISTRY:$IMAGE_TAG ."
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
                echo "Scanning Docker image with Trivy..."
                sh "trivy image $REGISTRY:$IMAGE_TAG || true"
            }
        }

        // 5. Deploy Stage
        stage('Deploy to Staging') {
            steps {
                echo "Deploying app to staging environment..."
                sh "docker run -d -p 3000:3000 --name ${APP_NAME}-staging $REGISTRY:$IMAGE_TAG"
            }
        }

        // 6. Release Stage
        stage('Release to Production') {
            when {
                branch 'main'
            }
            steps {
                echo "Releasing to production..."
                sh "docker tag $REGISTRY:$IMAGE_TAG $REGISTRY:latest"
                sh "docker push $REGISTRY:$IMAGE_TAG"
                sh "docker push $REGISTRY:latest"
            }
        }

        // 7. Monitoring Stage
        stage('Monitoring & Alerts') {
            steps {
                echo "Monitoring with Prometheus/Grafana..."
                echo "Simulating health check..."
                sh 'curl -f http://localhost:3000/health || exit 1'
                echo "Sending alert if app is down (placeholder for Datadog/New Relic integration)..."
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Cleaning up staging container..."
            sh "docker rm -f ${APP_NAME}-staging || true"
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs and alerts."
        }
    }
}
