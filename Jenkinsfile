pipeline {
    agent any

    environment {
        // Define SonarQube URL and token
        SONARQUBE_URL = 'http://localhost:9000/' // Replace with your SonarQube server URL
        SONARQUBE_TOKEN = 'sqa_e6ae6bba0ff58cbff5656b2d6d34fcc4fef5150d' // Replace with your SonarQube token ID in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git url: 'https://github.com/thefallen8/Just_A_Python_Repository.git', branch: 'main', credentialsId: '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' // Replace with your repository URL and branch
            }
        }


    stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube scan
                    withSonarQubeEnv('SonarQube_Server') { // Ensure 'SonarQube' matches the name configured in Jenkins
                        bat """C:\\Users\\Thehy\\Desktop\\Jithin\\Downloads\\sonar-scanner-cli-6.1.0.4477-windows-x64\\sonar-scanner-6.1.0.4477-windows-x64\\bin\\sonar-scanner.bat -Dsonar.projectKey=my-python-project -Dsonar.sources=. -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONARQUBE_TOKEN"""
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    try {
                        timeout(time: 30, unit: 'SECONDS') {
                            def qg = waitForQualityGate()
                            echo "Quality Gate status: ${qg.status}"
                            if (qg.status != 'OK') {
                                error "Pipeline aborted due to quality gate failure: ${qg.status}"
                            }
                        }
                    } catch (Exception e) {
                        echo "Error while waiting for quality gate: ${e.message}"
                        error "Pipeline aborted due to quality gate failure"
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
