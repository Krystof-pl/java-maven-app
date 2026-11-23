#!/user/bin/env groovy

library identifier: 'jenkins-share-library@main', retriever: modernSCM(
        [$class: 'GitSCMSource',
        remote: 'https://github.com/Krystof-pl/jenkins-share-library.git',
        credentialsId: '4855e374-9174-42c0-8940-1977a3494cef'
])

def gv

pipeline {
    agent any
    tools {
        maven "maven_3.10"
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                   buildJar()
                }
            }
        }
        stage("build and push image") {
            steps {
                script {
                    buildImage 'dockerkob/demo-app:jma-3.3'
                    dockerLogin()
                    dockerPush 'dockerkob/demo-app:jma-3.3'
                }
            }
        }
        stage("deploy") {
            when {
                expression { 
                    BRANCH_NAME == "main"
                 }
            }
            steps {
                script {
                    echo "deploying"
                    echo "github integration triggered"
                    gv.deployApp()
                }
            }
        }
    }   
}
