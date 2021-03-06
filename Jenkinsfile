pipeline {
    agent any
    environment {
        ENCRYPTED = credentials('decodebot')
    }
    stages {
        stage('Build') {
            steps {
                wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'XTerm']) {
                    sh 'bin/decrypt.sh && bin/clean.sh && bin/install.sh && bin/build.sh'
                }
            }
        }
        stage('Test') {
            steps {
                wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'XTerm']) {
                    sh 'bin/test.sh'
                }
                publishHTML([
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: false,
                    reportDir: 'build/coverage',
                    reportFiles: 'index.html', 
                    reportName: 'Coverage',
                    reportTitles: ''
                ])
            }
        }
        stage('Deploy') {
            steps {
                wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'XTerm']) {
                    sh 'bin/deploy.sh Continuous'
                }
            }
        }
    }
    post {
        success {
            slackSend (color: '#00FF00', message: "${env.JOB_NAME} - #${env.BUILD_NUMBER} Success after (${env.BUILD_URL})")
        }
        failure {
            slackSend (color: '#FF0000', message: "${env.JOB_NAME} - #${env.BUILD_NUMBER} Failure after (${env.BUILD_URL})")
        }
    }
}
