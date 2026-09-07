pipeline {
    agent any
    tools { nodejs 'node20' }
    stages {
        stage('Build') {
            steps {
                sh 'npm ci --omit=dev || echo "no package.json"'
            }
        }
        stage('Package') {
            steps {
                sh 'rm -rf "${WORKSPACE}_scan" && mkdir -p "${WORKSPACE}_scan"'
                sh 'tar czf "${WORKSPACE}_scan/app.tar.gz" -C "${WORKSPACE}" --exclude=.git .'
            }
        }
        stage('SSC Scan') {
            steps {
                withCredentials([string(credentialsId: 'API_KEY', variable: 'MDSSC_API_KEY')]) {
                    sh 'docker run --rm --volumes-from jenkins -e MDSSC_SERVER -e MDSSC_API_KEY -e SCAN_TIMEOUT=600 opswat/mdssc-scanner:latest "${WORKSPACE}_scan/app.tar.gz"'
                    sh 'rm -rf "${WORKSPACE}_scan"'
                }
            }
        }
    }
}