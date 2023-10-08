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
                snykSecurity failOnIssues: false, organisation: 'DevTraleski', projectName: 'JuiceShop Clone', snykInstallation: 'SnykPlugin', snykTokenId: 'Snyk'
            }
        }
    }
}