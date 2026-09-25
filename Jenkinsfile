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
                    def dockerCmd="docker-compose -f docker-compose.yaml up --detach"
                    echo 'deploying docker image to EC2...'
                    sshagent(credentials: ['ec2-server'], executable: '') {
                        sh "scp docker-compose.yaml ec2-user@18.217.105.101:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.217.105.101 '${dockerCmd}'"
                    }
                }
            }
        }
    }
}
