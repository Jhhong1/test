pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                retry(3) {
                    sh 'echo "retry"'
                }

                timeout(time: 3, unit: 'SECONDS') {
                    sh '''
                        echo "sleep"
                        sleep 4
                    '''
                    
                }
            }
        }
    }
}
