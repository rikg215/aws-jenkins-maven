#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/rikg215/jenkins-shared-library.git',
    credentialsId: 'github_pat'
    ]
)

pipeline {
    agent any
    tools {
        maven 'maven-tool'
    }
    environment {
        IMAGE_NAME = 'rik215/bootcamp-test:java-maven-1.0'
    }
    stages {
        stage('build app') {
            steps {
                echo 'building application jar...'
                buildJar()
            }
        }
        stage('build image') {
            steps {
                script {
                    echo 'building the docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    def dockerCmd = "docker rm -f my-app 2>/dev/null; docker run -p 3080:8080 -d --name my-app ${IMAGE_NAME}"
                    sshagent(credentials: ['ec2-server'], executable: '') {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.118.146.140 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
