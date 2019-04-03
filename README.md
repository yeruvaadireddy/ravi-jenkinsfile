# ravi-jenkinsfile
jenkinsfile
pipeline {
    agent {
        label 'SL202_win' 
    }
    stages {
        stage("Fetch repository") {
            steps {
                    git 'https://git.ceesiesdomain.nl/scm/rsd/test_automation.git'
                }
            }
        stage('Run test') {
            steps {
                bat 'cd d:/SL202_Data/workspace/Front-end-SwiftNL/Sanctie_Regressie_Workflows_WCM' 
                bat 'mvn clean test -f d:/SL202_Data/workspace/Front-end-SwiftNL/Sanctie_Regressie_Workflows_WCM/pom.xml -Dtest=TestRunner'
            }
        }
    }
    post {
        always {
            echo 'Test run completed'
            cucumber buildStatus: 'UNSTABLE', failedFeaturesNumber: 999, failedScenariosNumber: 999, failedStepsNumber: 3, fileIncludePattern: '**/*.json', skippedStepsNumber: 999
        }
        success {
            echo 'Successfully!'
        }
        failure {
            echo 'Failed!'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
    options {
        timeout(time: 60, unit: 'MINUTES')
    }
}
