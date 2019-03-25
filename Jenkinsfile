node {
  def img
  stage('Clone repository') {
    checkout scm
  }
  stage('Build image') {
    img = docker.build('dockersamples/voting_app', '.')
  }

  stage('Push images') {
    docker.withRegistry('https://dell-harbor.dell.ecore.af.smil.mil', 'harbor-credentials') {
    img.push()
    }
  }
}