pipeline {
    agent any

    stages {
        stage('Preparation') {
            steps {
                git 'https://github.com/DevTraleski/juiceshop-clone'
            }
        }
        stage('Snyk Dependency Scan') {
            steps{
                snykSecurity failOnIssues: false, organisation: 'DevTraleski', projectName: 'JuiceShop Clone', snykInstallation: 'SnykPlugin', snykTokenId: 'Snyk'
            }
        }
    }
}