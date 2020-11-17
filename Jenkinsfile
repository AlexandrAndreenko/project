#!groovy
properties([disableConcurrentBuilds()])

pipeline {
    agent { 
        label 'master'
        }
        
    triggers { pollSCM('* * * * *') }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    stages {
        stage("First step") {
            steps {
                sh 'hostname'
                sh 'uptime'
            }
        }
        stage('Install docker to target') {
            steps {
                ansiblePlaybook(
                    inventory: 'hosts.yaml',
                    playbook: 'deploy.yaml'
                )
            }
    }
}
