pipeline {
    agent any
    tools {
            maven 'Maven'
            jdk 'DEFAULT'
    }
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
        stage('Deploy to nexus') {
                    when {
                        allOf {
                            branch 'develop'
                            expression {
                                params.release == true
                            }
                        }
                    }
                    steps {
                        sh """
                            mvn -B deploy:deploy-file -Dversion=1.0.0 -Dfile="build/ui-bundle.zip" -DrepositoryId=community-deploy -Durl=https://nexus.comdev.psi.de/repository/tools
                        """
                    }
        }

    }
}
