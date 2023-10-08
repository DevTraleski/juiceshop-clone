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
                sh 'forever start app.ts'
            }
        }
        stage('OWASP ZAP DAST Scan') {
            steps{
                sh 'curl http://localhost:3000/'
            }
        }
    }
}