#!groovy
properties([disableConcurrentBuilds()])

pipeline {
    agent { 
        label 'master'
        }

    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    stages {
        stage("Check Host") {
            steps {
                sh 'hostname'
                sh 'uptime'
            }
        }
        stage('Install docker to target') {
            steps {
                ansiblePlaybook(
                    inventory: 'hosts.yaml',
                    playbook: 'deploy.yaml',
                    disableHostKeyChecking: true
                )
            }    
         }
    }
    post {
        success {
            slackSend (color: '#00FF00', message: "SUCCESSFULL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
}    

