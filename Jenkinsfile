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
                sh '''
                    ARTIFACT="${WORKSPACE}_scan"
                    rm -rf "$ARTIFACT"
                    mkdir -p "$ARTIFACT"
                    tar czf "$ARTIFACT/app.tar.gz" -C "$WORKSPACE" --exclude=.git .
                '''
            }
        }
        stage('SSC Scan') {
            steps {
                withCredentials([string(credentialsId: 'API_KEY', variable: 'MDSSC_API_KEY')]) {
                    sh '''
                        docker run --rm \
                          --volumes-from jenkins \
                          -e MDSSC_SERVER \
                          -e MDSSC_API_KEY \
                          -e SCAN_TIMEOUT=600 \