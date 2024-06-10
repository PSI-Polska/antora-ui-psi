def defaultVersionTag ="1.0.0"

pipeline {
    agent any

    parameters {
        validatingString(
            name: 'versionTag',
            description: 'Custom tag for pushing maven, npm and docker artifacts and/or running test application.',
            defaultValue: defaultVersionTag,
            regex: /^(?:[a-zA-Z0-9_][\.\_\-a-zA-Z0-9]{0,127})?$/,
            failedValidationMessage: "128 characters at most. " +
                "Lowercase and uppercase letters, digits, underscores, periods and dashes."
        )
        booleanParam(
            name: 'release',
            description: 'Release (available only from develop branch)',
            defaultValue: false
        )
    }

    stages {
        stage('Build') {
            steps {
                dir('./community-modules/angular') {
                    sh ('npm install')
                    sh ('gulp bundle')
                }
            }
        }

        stage('Create tag') {
            when {
                allOf {
                    branch 'develop'
                    expression {
                        params.release == true
                    }
                }
            }
            steps {
                script {
                    withCredentials(
                        [gitUsernamePassword(credentialsId: 'ad-service-user-psi', gitToolName: 'DEFAULT')]
                    ) {
                        sh """
                            git tag -fm 'Tagged by Jenkins' ${params.versionTag}
                            git push -f origin ${params.versionTag}
                        """
                    }
                }
            }
        }
    }
}
