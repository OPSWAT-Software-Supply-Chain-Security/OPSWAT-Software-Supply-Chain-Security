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
                        SCAN_DIR="${WORKSPACE}_scan"
                        rm -rf "$SCAN_DIR"
                        mkdir -p "$SCAN_DIR"
                        cp -r "$WORKSPACE"/. "$SCAN_DIR"/
                        rm -rf "$SCAN_DIR/.git"
        
                        docker run --rm \
                          --volumes-from jenkins \
                          -e MDSSC_SERVER \
                          -e MDSSC_API_KEY \
                          -e SCAN_TIMEOUT=600 \
                          opswat/mdssc-scanner:latest "$SCAN_DIR"
        
                        rm -rf "$SCAN_DIR"
                    '''
                }
            }
        }
    }
}