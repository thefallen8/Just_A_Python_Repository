pipeline {
    agent any
    environment {
        scannerHome = tool 'SonarQube Scanner'
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
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.pullrequest.key=${env.CHANGE_ID} -Dsonar.pullrequest.branch=${env.CHANGE_BRANCH} -Dsonar.pullrequest.base=${env.CHANGE_TARGET} -Dsonar.projectKey=com.efx.sonarscan -Dsonar.pullrequest.provider=github -Dsonar.pullrequest.github.repository=thefallen8/Just_A_Python_Repository -Dsonar.pullrequest.github.endpoint=https://api.github.com"
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
