#!groovy

pipeline {
    agent none
    stages {
        stage('Build .deb packages') {
        agent any   
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
                        userRemoteConfigs: [[credentialsId: 'chaitanya', \
                        url: 'https://chaitanya@git.contentserv.com/DevOps/Deployment/CS-Docker-Infrastructure']]]
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
