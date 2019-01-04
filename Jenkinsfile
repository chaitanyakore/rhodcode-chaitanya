#!groovy

pipeline {
    agent none
    stages {
        stage('Codeception') {
           agent any
               steps {
                emailext (
                    subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                      <p>Will deploy to ${params.DEPLOY_TARGET}</p>
                      <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                    to: "chaitanya.kore@contentserv.com"
                )
                

               dir('www') {
                    checkout poll: true, scm: [$class: 'SubversionSCM', filterChangelog: false, \
                        ignoreDirPropChanges: false, \
                        locations: [[credentialsId: '90e7e239-48b2-455a-a72e-68522c3e70fd', \
                        depthOption: 'infinity', ignoreExternalsOption: false, \
                        local: '.', remote: 'https://svn.contentserv.com/development/branches/features/CS18_FDEV-1130/']], \
                        workspaceUpdater: [$class: 'UpdateUpdater']]
               }
                
}
		}        

stage('Codeception test') {
            agent any
            steps {
        sh 'cd www/admin.test && make test-all'
        }
        }
}

post {
        success {
            emailext (
                subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                  <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                to: "chaitanya.kore@contentserv.com"
            )
        }
        failure {
            emailext (
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                  <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                to: "chaitanya.kore@contentserv.com"
            )
        }
    }
}
