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
    stages {
        stage('Extract and Concatenate Payload Information') {
            steps {
                script {
                    // Assuming the payload is obtained from an environment variable or some source
                    def payload = readJSON(text: env.PAYLOAD)

                    // Extract the branch name from ref
                    def ref = payload.ref
                    def branch = ref.substring(ref.indexOf('feature'))

                    // Extract SVN URL
                    def svnUrl = payload.repository.svn_url

                    // Concatenate the values
                    def concatenatedResult = "${svnUrl}/tree/${branch}"

                    // Print the result to the console
                    echo "Concatenated Result: ${concatenatedResult}"
                }
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
