pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'release',
            description: 'Release (available only from develop branch)',
            defaultValue: false
        )
    }

    stages {
        stage('Build') {
            steps {
                  sh ('npm install -g gulp-cli')
                  sh ('npm install')
                  sh ('gulp bundle')
            }
        }
        stage('Publish') {
            when {
                allOf {
                    branch 'develop'
                    expression {
                        params.release == true
                    }
                }
            }
            steps {
                withCredentials([string(credentialsId: 'npm-config-token', variable: 'NPM_TOKEN')]){
                    sh('npm publish')
                }
            }
        }
    }
}
