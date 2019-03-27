node {
  withEnv(['export DOCKER_CONTENT_TRUST=1',
  'DOCKER_CONTENT_TRUST_SERVER=https://dell-harbor.dell.ecore.af.smil.mil:4443'
  'export DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE=VMware1!',
  'export DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE=VMware1!']){
    def img
    stage('Clone repository') {
      checkout scm
    }
    stage('Build image') {
      sh 'printenv'
      img = docker.build('sdtf/vote_app', '.')
    }
    stage('Push image') {
      docker.withRegistry('https://dell-harbor.dell.ecore.af.smil.mil', 'harbor-credentials') {
      img.push()
      }
    }
  }
}