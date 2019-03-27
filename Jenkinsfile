node {
  def img
  stage('Clone repository') {
    checkout scm
  }
  stage('Build image') {
    img = docker.build('sdtf/vote_app', '.')
  }
  stage('Push image') {
    docker.withRegistry('https://dell-harbor.dell.ecore.af.smil.mil', 'harbor-credentials') {
    img.push()
    }
  }
}