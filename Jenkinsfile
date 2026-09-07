pipeline {
    agent any
    tools { nodejs 'node20' }
    stages {
        stage('Check') {
            steps {
                sh 'node -v'
                sh 'npm -v'
                sh 'ls -la'
            }
        }
        stage('SSC Scan') {
            steps {
                withCredentials([string(credentialsId: 'API_KEY', variable: 'MDSSC_API_KEY')]) {
                    sh '''
                        docker run --rm \
                          -e MDSSC_SERVER \
                          -e MDSSC_API_KEY \
                          -e FAIL_ON_VULNERABILITIES=true \
                          -e VULNERABILITY_THRESHOLD=high \
                          -e SCAN_TIMEOUT=600 \
                          -v "$WORKSPACE":/scan \
                          opswat/mdssc-scanner:latest
                    '''
                }
            }
        }
    }
}