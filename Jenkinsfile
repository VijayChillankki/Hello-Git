
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
              string(name: 'SAFARI_CYCLE_ID', defaultValue: '2478398', description: 'The Cycle ID is used for safari test results within the QTest Platform. This number typically differs per release.'),
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

pipeline {
    agent any
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
            agent {
                dockerfile {
                    filename 'Dockerfile'
                    dir 'jenkins'
                }
            }
            environment {
              HOME = '.'
            }
            steps {
                script {
                    web.each { entity ->
                        stage (entity.key) {
                            if (entity.value) {
                                echo "$entity.key browser executed"
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
                            }
                        }
                    }
                }
            }
        }
    }
}
