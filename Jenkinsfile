#!/usr/bin/env groovy
def repo= "https://gitea.mahillmann.de/Marcel/hub"

pipeline {
    agent any
    environment {
        CI=1
        GOPATH="${WORKSPACE}"
        GOPROXY="https://nexus.mahillmann.de/repository/goproxy/"
    }
    options {
        // parallelsAlwaysFailFast()
        ansiColor('xterm')
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '1', numToKeepStr: '7')
        // disableConcurrentBuilds()
        disableResume()
    } // options
    stages {
        stage('go 1.9'){
            steps{
                node(label: 'docker'){
                    checkout([ $class: 'GitSCM'
                             , branches: [[name: '*/master']]
                             , doGenerateSubmoduleConfigurations: false
                             , extensions: [ [ $class: 'RelativeTargetDirectory'
                                             , relativeTargetDir: 'src/github.com/github/hub' ]
                                           ]
                             , gitTool: 'Default'
                             , submoduleCfg: []
                             , userRemoteConfigs: [[credentialsId: 'github-com', url: repo]]
                             ])
                    withEnv(["CI=","path+go=${tool(name: '1.9', type: 'go')}/bin"]) {
                        script {
                            try{
                                dir('src/github.com/github/hub'){
                                    sh 'make && make test-all'
                                }
                            }finally{
                                sh(script:'find . -delete',returnStdout: true)
                                currentBuild.result = 'SUCCESS'
                            }
                        }

                    }// withEnv
                } // node
            } // steps
        } // stage
        stage('go 1.10'){
            steps{
                node(label: 'docker'){
                    checkout([ $class: 'GitSCM'
                             , branches: [[name: '*/master']]
                             , doGenerateSubmoduleConfigurations: false
                             , extensions: [ [ $class: 'RelativeTargetDirectory'
                                             , relativeTargetDir: 'src/github.com/github/hub' ]
                                           ]
                             , gitTool: 'Default'
                             , submoduleCfg: []
                             , userRemoteConfigs: [[credentialsId: 'github-com', url: repo]]
                             ])
                    withEnv(["path+go=${tool(name: '1.10', type: 'go')}/go/bin"]) {
                        script {
                            try{
                                dir('src/github.com/github/hub'){
                                    sh 'make && make test-all'
                                }
                            }finally{
                                sh(script:'find . -delete',returnStdout: true)
                            }
                        }

                    }// withEnv
                }
            }// steps
        }// go 1.10
        stage('go 1.11'){
            steps{
                node(label: 'docker'){
                    checkout([ $class: 'GitSCM'
                             , branches: [[name: '*/master']]
                             , browser: [$class: 'GithubWeb', repoUrl: 'https://github.com/MarcelHillmann/hub']
                             , doGenerateSubmoduleConfigurations: false
                             , extensions: []
                             , gitTool: 'Default'
                             , submoduleCfg: []
                             , userRemoteConfigs: [[credentialsId: 'github-com', url: repo]]])
                    withEnv(["path+go=${tool(name: '1.11', type: 'go')}/bin"]) {
                        script{
                            runScript(false)
                        }
                    }// withEnv
                }
            } // steps
        }// go 1.11
        stage('go 1.12'){
            steps{
                node(label: 'docker'){
                    checkout([ $class: 'GitSCM'
                             , branches: [[name: '*/master']]
                             , browser: [$class: 'GithubWeb', repoUrl: 'https://github.com/MarcelHillmann/hub']
                             , doGenerateSubmoduleConfigurations: false
                             , extensions: []
                             , gitTool: 'Default'
                             , submoduleCfg: []
                             , userRemoteConfigs: [[credentialsId: 'github-com', url: repo]]])
                    withEnv(["path+go=${tool(name: '1.12', type: 'go')}/bin"]) {
                        script{
                            runScript(false)
                        }
                    }// withEnv
                }
            }// steps
        }// go 1.12
        stage('go 1.13'){
            steps{
                node(label: 'docker'){
                    checkout([ $class: 'GitSCM'
                             , branches: [[name: '*/master']]
                             , browser: [$class: 'GithubWeb', repoUrl: 'https://github.com/MarcelHillmann/hub']
                             , doGenerateSubmoduleConfigurations: false
                             , extensions: []
                             , gitTool: 'Default'
                             , submoduleCfg: []
                             , userRemoteConfigs: [[credentialsId: 'github-com', url: repo]]])
                    withEnv(["path+go=${tool(name: '1.13', type: 'go')}/bin"]) {
                        script{
                            runScript(true, false)
                        }
                    }// withEnv
                }
            }// steps
        }// go 1.13
    } // stages
} // pipeline

def runScript(def cucumber=false, def clean = true){
    try{
        sh 'make && make test-all'
    }finally{
        if(cucumber){
            dir('src/github.com/github/hub'){
                cucumber failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: 'cucumber.json', jsonReportDirectory: 'bin/', pendingStepsNumber: -1, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1
            }
        }
        if( clean ){
            sh(script:'find . -delete',returnStdout: true)
        }
    }
}