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
    }
}