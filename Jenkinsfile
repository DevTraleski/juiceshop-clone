pipeline {
    agent any

    tools {
        nodejs "NodePlugin"
    }

    stages {
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv(credentialsId: 'JenkinsTokenSonar', installationName: 'SonarQubeScanner') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}