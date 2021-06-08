properties([
    parameters([
              booleanParam(name: 'Refresh', defaultValue: true, description: 'Read Jenkinsfile and exit.'),
              separator(name: 'WEB_BROWSERS', sectionHeader: 'Web Browsers'),
              booleanParam(name: 'chrome', defaultValue: true, description: 'Controls whether to run tests on Chrome Browser or not'),
              booleanParam(name: 'edge', defaultValue: true, description: 'Controls whether to run tests on Edge Browser or not'),
              booleanParam(name: 'safari', defaultValue: true, description: 'Controls whether to run tests on Safari Browser or not'),
              separator(name: 'MOBILE_BROWSERS', sectionHeader: 'Mobile Browsers'),
              booleanParam(name: 'iphone', defaultValue: true, description: 'Controls whether to run tests on iPhone Browser or not'),
              booleanParam(name: 'galaxys20', defaultValue: true, description: 'Controls whether to run tests on GalaxyS20 Browser or not'),
              booleanParam(name: 'ipad', defaultValue: true, description: 'Controls whether to run tests on iPad Browser or not'),
              separator(name: "end"),
              string(name: 'BRANCH', defaultValue: 'main', description: 'The branch from which to run the tests.'),
              string(name: 'CHROME_QTEST_CYCLE_ID', defaultValue: '2478396', description: 'The Cycle ID is used for chrome test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'EDGE_QTEST_CYCLE_ID', defaultValue: '2478397', description: 'The Cycle ID is used for edge test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'SAFARI_QTEST_CYCLE_ID', defaultValue: '2478398', description: 'The Cycle ID is used for safari test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'IPHONE_QTEST_CYCLE_ID', defaultValue: '2478396', description: 'The Cycle ID is used for iPhone test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'GALAXY20_QTEST_CYCLE_ID', defaultValue: '2478397', description: 'The Cycle ID is used for galaxyS20 test results within the QTest Platform. This number typically differs per release.'),
              string(name: 'IPAD_QTEST_CYCLE_ID', defaultValue: '2478398', description: 'The Cycle ID is used for iPad test results within the QTest Platform. This number typically differs per release.'),
    ]) 
])


def web = [
    chrome: params.chrome,
    edge: params.edge,
    safari: params.safari,
]

def mob = [
    iphone: params.iphone,
    galaxys20: params.galaxys20,
    ipad: params.ipad
]

def PROJECT_ID = '91839'

pipeline {
    agent {
        dockerfile {
        filename 'Dockerfile'
        dir 'jenkins'
        }
    }
    environment {
              HOME = '.'
        }
    stages {
        stage('Read Jenkinsfile') {
            when {
                expression { params.Refresh == true }
            }
            steps {
                echo "Jenkinsfile reloaded successfully"
            }
        }
        stage('Execute Web Browsers') {
            steps {
                script {
                    web.each { entity ->
                        stage (entity.key) {
                            if (entity.value) {
                                echo "$entity.key browser executed"
                                sleep 5
                                dir(env.WORKSPACE) {
                                sh "echo -DBrowserType=browserstack_'${entity.key}' -Dtestng.report.xml.name=testng-result-${entity.key}.xml"
                                sh " echo npm cache clean  --force"
                                def browser = "${entity.key}".toUpperCase()
                                def var_testCycleId = "params.${browser}_QTEST_CYCLE_ID"
                                def testCycleId = params."${browser}_QTEST_CYCLE_ID"
                                sh "echo node delivery.js projectid=${PROJECT_ID} cycleid=${testCycleId} testngresultxml=testng-result-${entity.key}.xml"
                                //sh "echo mv src/test/resources/Reports/Extent Report.html src/test/resources/Reports/Extent-Report-${entity-key}.html"
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Execute Mobile Browsers') {
            steps {
                script {
                    mob.each { entity ->
                        stage (entity.key) {
                            if (entity.value) {
                                echo "$entity.key mobile browser executed"
                                sleep 5
                                dir(env.WORKSPACE) {
                                sh "echo -DBrowserType=browserstack_'${entity.key}' -Dtestng.report.xml.name=testng-result-${entity.key}.xml"
                                sh " echo npm cache clean  --force"
                                def browser = "${entity.key}".toUpperCase()
                                def var_testCycleId = "params.${browser}_QTEST_CYCLE_ID"
                                def testCycleId = params."${browser}_QTEST_CYCLE_ID"
                                sh "echo node delivery.js projectid=${PROJECT_ID} cycleid=${testCycleId} testngresultxml=testng-result-${entity.key}.xml"
                                //sh "echo mv src/test/resources/Reports/Extent Report.html src/test/resources/Reports/Extent-Report-${entity-key}.html"
                            }
                        }
                    }
                }
            }
        }
    }
    post {
    always {
        sh "echo See browserstack execution details at https://automate.browserstack.com/dashboard/"
        //archiveArtifacts artifacts: 'src/test/resources/Reports/Extent-*.html'
        cleanWs()
    }
}
}
