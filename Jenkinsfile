pipeline {
    agent any

    tools {
        nodejs "NodePlugin"
    }

    stages {
        stage('Secret Management') {
            steps{
                withCredentials([string(credentialsId: 'SecretFlag', variable: 'flag')]) {
                    sh 'echo ${flag} > ctf.key'
                    sh 'cat ctf.key'
                }
            }
        }
        stage('Install Dependencies'){
            steps{
                sh 'npm install' // Dependency Installation stage
            }
        }
        stage('Snyk Dependency Scan') {
            steps{
                snykSecurity failOnIssues: false, organisation: 'devtraleski', projectName: 'JuiceShop', snykInstallation: 'SnykPlugin', snykTokenId: 'Snyk'
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
        stage('OWASP ZAP DAST Scan') {
            steps{
                sh 'nohup npm start &'
                sshagent(['zapssh']) {
                    sh 'ssh -o StrictHostKeyChecking=no zap@172.17.0.2 "python /zap/zap-baseline.py -t http://172.17.0.3:3000" || true'
                }
            }
        }
    }
}