stage('Build') {
    steps {
        sh 'npm ci --omit=dev'
        sh 'npm run build || echo "no build script"'
    }
}
stage('Package') {
    steps {
        sh '''
            ARTIFACT="${WORKSPACE}_scan"
            rm -rf "$ARTIFACT" && mkdir -p "$ARTIFACT"
            tar czf "$ARTIFACT/app.tar.gz" \
              -C "$WORKSPACE" \
              --exclude=.git \
              --exclude="*.tar.gz" \
              .
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
                  opswat/mdssc-scanner:latest "${WORKSPACE}_scan/app.tar.gz"
            '''
            sh 'rm -rf "${WORKSPACE}_scan"'
        }
    }
}