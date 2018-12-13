#!groovy

pipeline {
    agent none
    stages {
        stage('Build .deb packages') {
            agent {
                dockerfile {
                    dir 'deb-packager'
                }
            }
            steps {
                emailext (
                    subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                      <p>Will deploy to ${params.DEPLOY_TARGET}</p>
                      <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                    to: "chaitanya.kore@contentserv.com"
                )
                
                dir('test') {
                    checkout poll: false, scm: [$class: 'GitSCM', \
                        branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, \
                        extensions: [], submoduleCfg: [], \
                        userRemoteConfigs: [[credentialsId: 'dfa6f5a3-608c-4f05-bb23-3c096c4cc430', \
                        url: 'https://mrjenkins@git.contentserv.com/DevOps/Deployment/CS-Docker-Infrastructure']]]
                }
                
                dir('www') {
                    checkout poll: true, scm: [$class: 'SubversionSCM', filterChangelog: false, \
                        ignoreDirPropChanges: false, \
                        locations: [[credentialsId: '90e7e239-48b2-455a-a72e-68522c3e70fd', \
                        depthOption: 'infinity', ignoreExternalsOption: false, \
                        local: '.', remote: 'https://svn.contentserv.com/development/branches/CS18/CS18.0/admin']], \
                        workspaceUpdater: [$class: 'UpdateUpdater']]
                }
}
}


stage('Submit to APT Repository') {
            agent any
            steps {
                sh 'touch test1'
                sh 'cp -f test1 /mnt/Docker-push-dir/'
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
