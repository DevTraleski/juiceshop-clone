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
        stage('Run Site'){
            steps{
                sh 'npm start'
            }
        }
        stage('OWASP ZAP DAST Scan') {
            steps{
                sshagent(['zapssh']) {
                    sh 'ssh -o StrictHostKeyChecking=no zap@172.17.0.4 "python /zap/zap-baseline.py -t http://172.17.0.2:3000"'
                }
            }
        }
    }
}