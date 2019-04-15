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
    sh 'curl http://www.sourcecertain.com/img/Example.png -o example.png'
    archiveArtifacts artifacts: '*.png', fingerprint: true
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
