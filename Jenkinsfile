pipeline {
    agent any
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/nadhirah5/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            environment {
                SONAR_TOKEN = credentials('sq') // Reference the credentials ID for SonarQube token
            }
            steps {
                script {
                    def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation';
                    // Print the path of SonarQube scanner
                    sh "echo SonarQube Scanner Path: ${scannerHome}"

                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://172.28.136.132:9000 -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }
    }
    post {
        always {
            recordIssues enabledForFailure: true, tools: [sonarQube()]
        }
    }
}
