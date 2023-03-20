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
        IMAGE_NAME = 'sulemankhan/react-nodejs-app:1.3'
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
                    def shellCmd = "bash ./deploy.sh ${env.IMAGE_NAME}"
                    def ec2Instance = "ec2-user@15.206.164.87"
                    sshagent(['ec2-server-key']) {
                        sh "scp deploy.sh ${ec2Instance}:/home/ec2-user"
                        sh "scp docker-compose.yml ${ec2Instance}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} '${shellCmd}' "
                    }
                }
            }
        }
    }
}
