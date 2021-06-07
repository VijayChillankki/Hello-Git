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
                            }
                        }
                    }
                }
            }
        }
    }
}
