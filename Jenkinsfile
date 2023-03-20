#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                script {
                    echo 'Testing the application..'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the application..'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -d -p 3000:3080 sulemankhan/react-nodejs-app:1.1'

                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@15.206.164.87 '${dockerCmd}' "
                    }
                }
            }
        }
    }
}
