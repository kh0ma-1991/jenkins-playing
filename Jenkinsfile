#!/usr/bin/env groovy
properties([
        buildDiscarder(
                logRotator(
                        artifactDaysToKeepStr: '',
                        artifactNumToKeepStr: '10',
                        daysToKeepStr: '',
                        numToKeepStr: '15')),
        disableConcurrentBuilds(),
        pipelineTriggers([
                pollSCM('H/5 * * * *')
        ])
])

node {
  checkout scm
  stage('DOWNLOAD FILE') {
    sh "echo ${currentBuild.getFullProjectName() + env.BUILD_NUMBER} > build_info.txt"
    sh "date > current_date.txt"
    archiveArtifacts artifacts: '*.txt', fingerprint: true
  }
  stage('Trigger downstream') {
    build   job: "MB2/${env.BRANCH_NAME}",
            parameters: [
                    [$class: 'StringParameterValue', name: 'UpstreamJobName', value: "${currentBuild.getFullProjectName()}"],
                    [$class: 'StringParameterValue', name: 'UpstreamBuildNumber', value: "${env.BUILD_NUMBER}"]
            ],
            wait: false
  }
}
