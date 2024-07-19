pipeline {
    agent any
    environment {
        scannerHome = tool 'SonarQube Scanner'
        // Define SonarQube URL and token
        SONARQUBE_URL = 'http://localhost:9000/' // Replace with your SonarQube server URL
        SONARQUBE_TOKEN = 'sqa_e6ae6bba0ff58cbff5656b2d6d34fcc4fef5150d' // Replace with your SonarQube token ID in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube_Server') {
                        bat "sonar-scanner.bat -Dsonar.projectKey=my-python-project -Dsonar.sources=. -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONARQUBE_TOKEN"
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
    }
}
