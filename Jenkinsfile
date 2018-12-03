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
    }
}
post {

success{

emailext (
subject: "Job Successfull "
body: "Jenkins Test"
to: "chaitanya.kore@contentserv.com"                
)
}

failure {
emailext (
subject: "Job Failure "
body: "Jenkins Test"
to: "chaitanya.kore@contentserv.com"
}
}
