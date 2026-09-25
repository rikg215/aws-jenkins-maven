#!/usr/bin.env groovy

pipeline {   
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application..."

                }
            }
        }
        stage("build") {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    def dockerCmd = 'docker run -p 3080:3080 -d rik215/bootcamp-test:my-app-1.0'
                    sshagent(credentials: ['ec2-server'], executable: '') {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.118.146.140 ${dockerCmd}"
                    }
                }
            }
        }               
    }
} 
