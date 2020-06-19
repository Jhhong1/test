@Library('alauda-cicd') _
def APP
pipeline {
    agent any
    parameters {
        string(name: 'COMPONENT', defaultValue: '', description: 'The component of the chart worked on. Will be used to update the values.yaml file.')
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to be used by the chart.')
    }
    stages {
        stage('Clone') {
            steps {
                script {
                    container('tools') {
                        APP = checkout scm
                        echo "current branch: ${APP.GIT_BRANCH}"
                        release = deploy.chartsRelease(
							params.COMPONENT,
							params.BRANCH
							)
                       release.setScm(scmVars)
                    }
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
                            caseType: release.component,
                            branch: branch,
                            pipeline: "hhjin",
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
