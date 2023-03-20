#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://gitlab.com/sulemank97/jenkins-shared-library.git',
     credentialsId: 'suleman-gitlab'
    ]
)

pipeline {
    agent any
    environment {
        IMAGE_NAME = 'sulemankhan/react-nodejs-app:1.2'
    }
    stages {
        stage('Test') {
            steps {
                script {
                    echo 'Testing the application..'
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    buildImage env.IMAGE_NAME
                    dockerLogin()
                    dockerPush env.IMAGE_NAME
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                    def dockerCmd = "docker run -d -p 3000:3080 ${env.IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@15.206.164.87 '${dockerCmd}' "
                    }
                }
            }
        }
    }
}
