pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "start build"'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "start test"'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "start deploy"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '*.xml', fingerprint: true
            junit '*.xml'
        }
    }
}
