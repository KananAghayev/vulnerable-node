pipeline {
    agent any
    tools {
        nodejs 'NodeJS' // Use configured Node.js
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token') // Reference Snyk API token
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/KananAghayev/vulnerable-node.git', branch: 'master'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Create node_modules to fix previous error
            }
        }
        stage('Snyk Test') {
            steps {
                snykSecurity(
                    snykInstallation: 'snyk_test', // Snyk CLI installation name
                    snykTokenId: 'snyk-token',    // Credential ID
                    severity: 'low',              // Use 'severity' for compatibility
                    failOnIssues: true,           // Fail build on vulnerabilities
                    additionalArguments: '--json' // JSON output
                )
            }
        }
        stage('Snyk Monitor') {
            steps {
                snykSecurity(
                    snykInstallation: 'snyk_test',
                    snykTokenId: 'snyk-token',
                    monitorProjectOnBuild: true   // Enable monitoring
                )
            }
        }
        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: 'snyk_report.json', allowEmptyArchive: true
            }
        }
    }
    post {
        success {
            echo 'Snyk scan completed successfully.'
        }
        failure {
            echo 'Snyk scan failed. Check logs for details.'
        }
    }
}
