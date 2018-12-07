pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    

stage('docker checkout') {
steps{

dir('test') {
                    checkout poll: false, scm: [$class: 'GitSCM', \
                        branches: [[name: '*/CS18.0']], doGenerateSubmoduleConfigurations: false, \
                        extensions: [], submoduleCfg: [], \
                        userRemoteConfigs: [[credentialsId: 'dfa6f5a3-608c-4f05-bb23-3c096c4cc430', \
                        url: 'https://mrjenkins@git.contentserv.com/DevOps/Deployment/CS-Docker-Infrastructure']]]



}
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
