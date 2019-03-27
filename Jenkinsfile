node {
  withEnv(['DOCKER_CONTENT_TRUST=0',
  'DOCKER_CONTENT_TRUST_SERVER=https://dell-harbor.dell.ecore.af.smil.mil:4443',
  'DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE=VMware1!',
  'DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE=VMware1!']){
    def img
    stage('Clone repository') {
      checkout scm
    }
    stage('Build image') {
      img = docker.build('sdtf/vote_app:latest', '.')
    }
    withEnv(['DOCKER_CONTENT_TRUST=1']){
    stage('Push image') {
      docker.withRegistry('https://dell-harbor.dell.ecore.af.smil.mil', 'harbor-credentials') {
      img.push()
      }
    }
  }
  }
}