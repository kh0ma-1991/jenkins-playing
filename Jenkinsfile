node {
  checkout scm
  stage('DOWNLOAD FILE') {
    sh 'curl http://www.sourcecertain.com/img/Example.png -o example.png'
    archiveArtifacts artifacts: '*.png', fingerprint: true
  }
  stage('Trigger downstream') {
    build job: "MB2${env.BRANCH_NAME}", wait: false
  }
}
