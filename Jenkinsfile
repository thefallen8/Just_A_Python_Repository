pipeline {
    agent any

triggers {
        githubPush() // Trigger on GitHub push events
    }


    environment {
        // Define SonarQube URL
        SONARQUBE_URL = 'http://localhost:9000/' // Replace with your SonarQube server URL
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git url: 'https://github.com/thefallen8/Just_A_Python_Repository.git', branch: 'main', credentialsId: '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' // Replace with your repository URL and branch
            }
        }


                stage('Extract Branch Name') {
            steps {
                script {
                    // Extract the branch name from the webhook payload
                    def ref = "${GIT_COMMIT_REF}"
                    if (ref.startsWith('refs/heads/')) {
                        env.BRANCH_NAME = ref.replaceFirst('refs/heads/', '')
                    } else {
                        env.BRANCH_NAME = 'main'
                    }
                    echo "Branch Name: ${env.BRANCH_NAME}"
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Retrieve SonarQube token from Jenkins credentials store
                    withCredentials([string(credentialsId: 'SonarQube_Jenkis_Token', variable: 'SONARQUBE_TOKEN')]) { // Replace 'sonar-token-id' with your Jenkins credentials ID
                        // Run SonarQube scan
                        withSonarQubeEnv('SonarQube_Server') { // Ensure 'SonarQube_Server' matches the name configured in Jenkins
                            bat """C:\\Users\\Thehy\\Desktop\\Jithin\\Downloads\\sonar-scanner-cli-6.1.0.4477-windows-x64\\sonar-scanner-6.1.0.4477-windows-x64\\bin\\sonar-scanner.bat -Dsonar.projectKey=my-python-project -Dsonar.sources=. -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONARQUBE_TOKEN"""
                        }
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
