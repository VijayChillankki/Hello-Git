node {
    stage ('SCM checkout'){
        checkout scm
    }
}
properties([
    parameters([
              separator(name: "WEB_BROWSERS", sectionHeader: "Web Browsers"),
              booleanParam(name: 'chrome', defaultValue: true, description: 'Controls whether to run tests on Chrome Browser or not'),
              booleanParam(name: 'edge', defaultValue: true, description: 'Controls whether to run tests on Edge Browser or not'),
              booleanParam(name: 'safari', defaultValue: true, description: 'Controls whether to run tests on Safari Browser or not'),
              separator(name: "MOBILE_BROWSERS", sectionHeader: "Mobile Browsers"),
              booleanParam(name: 'iphone', defaultValue: true, description: 'Controls whether to run tests on iPhone Browser or not'),
              booleanParam(name: 'galaxys20', defaultValue: true, description: 'Controls whether to run tests on GalaxyS20 Browser or not'),
              booleanParam(name: 'ipad', defaultValue: true, description: 'Controls whether to run tests on iPad Browser or not'),
              separator(name: "end"),
              string(name: 'BRANCH', defaultValue: 'main', description: 'The branch from which to run the tests.'),
              string(name: 'CHROME_QTEST_CYCLE_ID', defaultValue: '2478396', description: 'The Cycle ID is used for chrome test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'EDGE_QTEST_CYCLE_ID', defaultValue: '2478397', description: 'The Cycle ID is used for edge test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'SAFARI_CYCLE_ID', defaultValue: '2478398', description: 'The Cycle ID is used for safari test results within the QTest Platform. This number typically differs per release.'),
    ])
])

def PROJECTID = '91839'

pipeline {
    agent none
    stages {
        stage ('Build'){
            agent {
                dockerfile {
                    filename 'Dockerfile'
                    dir 'jenkins'
                }
            }
            environment {
              HOME = '.'
            }
            steps("build") {
                dir(env.WORKSPACE) {
                       sh "mvn clean test -DBrowserType=browserstack_'${params.BROWSER_TYPE}'"	
                }
            }
            post {
                always {
                    sh "echo See browserstack execution details at https://automate.browserstack.com/dashboard/"
                    archiveArtifacts artifacts: 'src/test/resources/Reports/Extent Report.html'
                    publishHTML (target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target/surefire-reports/',
                        reportFiles: 'index.html',
                        reportName: "CBC Automation Report"
                    ])
                    sh "npm cache clean  --force"
                    def testcycleid = getTestCycleIDbyBrowserType(${params.BROWSER_TYPE})
                    sh "node delivery.js projectid=${PROJECTID} cycleid=${testcycleid}"
                    cleanWs()
                }
            }
        }
    }
}

    def getTestCycleIDbyBrowserType(BROWSER_TYPE) {
    if (BROWSER_TYPE == 'chrome') {
        return ${params.CHROME_QTEST_CYCLE_ID}
    }
    else if (BROWSER_TYPE == 'edge') {
        return ${params.EDGE_QTEST_CYCLE_ID}
    }
    else {
        return ${params.SAFARI_CYCLE_ID}
        }
    }
