pipeline {
    agent any

    stages {
        stage('Snyk Dependency Scan') {
            snykSecurity failOnIssues: false, organisation: 'DevTraleski', projectName: 'JuiceShop Clone', snykInstallation: 'SnykPlugin', snykTokenId: 'Snyk'
        }
    }
}