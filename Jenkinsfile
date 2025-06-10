pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/KananAghayev/vulnerable-node.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Security Scan') {
            steps {
                snykSecurity(
                    snykInstallation: 'snyk_test',
                    snykTokenId: 'your-snyk-token',
                    severity: 'low'
                )
            }
        }
    }
}
