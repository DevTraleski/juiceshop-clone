pipeline {
    agent any

    tools {
        nodejs "NodePlugin"
    }

    stages {
        stage('Akeyless Secret Management') {
            def secrets = [
                [path: '/secretflag', engineVersion: 1, secretValues: [
                        [envVar: 'flag', vaultKey: 'flag']
                    ]
                ]
            ]
            def configuration = [vaultUrl: 'https://hvp.akeyless.io',
                         vaultCredentialId: 'p-l3s2gxuuj2k1',
                         engineVersion: 1]
            steps{
                withVault([configuration: configuration, vaultSecrets: secrets]) {
                    sh 'echo $flag'
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
                    sh 'ssh -o StrictHostKeyChecking=no zap@172.17.0.4 "python /zap/zap-baseline.py -t http://172.17.0.2:3000" || true'
                }
            }
        }
    }
}