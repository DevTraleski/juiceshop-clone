pipeline {
    agent any

    tools {
        nodejs "NodePlugin"
    }

    stages {
        stage('Install Dependencies'){
            steps{
                sh 'npm install' // Dependency Installation stage
            }
        }
        stage('Snyk Dependency Scan') {
            steps{
                snykSecurity failOnIssues: false, organisation: 'devtraleski', projectName: 'JuiceShop Clone', snykInstallation: 'SnykPlugin', snykTokenId: 'Snyk'
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv(credentialsId: 'JenkinsTokenSonar', installationName: 'SonarQubeScanner') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=JuiceShop"
                }
            }
        }
    }
}