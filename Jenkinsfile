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
                
                dir('Docker-18.0') {
                    checkout poll: false, scm: [$class: 'GitSCM', \
                        branches: [[name: '*/CS18.0']], doGenerateSubmoduleConfigurations: false, \
                        extensions: [], submoduleCfg: [], \
                        userRemoteConfigs: [[credentialsId: 'dfa6f5a3-608c-4f05-bb23-3c096c4cc430', \
                        url: 'https://mrjenkins@git.contentserv.com/DevOps/Deployment/CS-Docker-Infrastructure']]]
                }
                }
		}

stage('Build Docker Images') {
            agent any
            steps {
                sh 'touch test1'
                sh 'cp -f test1 /mnt/Docker-push-dir/'
        sh 'cd Docker-18.0 && make build-all'
        }
        }

stage('Submit Docker Images to Docker registry') {
            agent any
            steps {
	sh 'cat /Jenkins-CI/cs_docker__password.txt | docker login cs-docker.contentserv.com --username cschaitanya --password-stdin '
        sh 'docker push cs-docker.contentserv.com/dev/cs-ubuntu-php:CS18.0'
	sh 'docker push cs-docker.contentserv.com/dev/cs-centos-php:CS18.0'
	sh 'docker push cs-docker.contentserv.com/dev/cs-mariadb:CS18.0'
	sh 'docker push cs-docker.contentserv.com/dev/cs-mysql:CS18.0'
	sh 'docker push cs-docker.contentserv.com/test/cs-mariadb-test:CS18.0'
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
