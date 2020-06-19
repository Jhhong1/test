@Library('alauda-cicd') _
def APP
pipeline {
    agent any
    parameters {
        string(name: 'COMPONENT', defaultValue: '', description: 'The component of the chart worked on. Will be used to update the values.yaml file.')
    }
    stages {
        stage('Clone') {
            steps {
                script {
                    APP = checkout scm
                    BRANCH = ${APP.GIT_BRANCH}
                    echo "current branch: ${BRANCH}"
                }
            }
        }
        stage('Deploy') {
            steps {
                retry(3) {
                    sh 'echo "retry"'
                }

                timeout(time: 3, unit: 'SECONDS') {
                    sh '''
                        echo "sleep"
                        sleep 2
                    '''

                }
            }
        }
        stage('Trigger') {
            steps {
                script {
                    container('tools') {
                        deploy.triggerAuto([
                            env: 'staging',
                            caseType: params.COMPONENT,
                            branch: ${BRANCH},
                            pipeline: "hhjinc",
                        ]).addPostAction({ succeeded ->
                            if (!succeeded) {
                                echo "API Testing Failed, will rollback environment..."
                                return "Automated test failed..."
                            }
                            return ""
                        }).start()
                    }
                }
            }
        }
    }
}
