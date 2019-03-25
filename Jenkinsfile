node {
  def img
  stage('Clone repository') {
    checkout scm
  }
  stage('Build voting app image') {
    img = docker.build('dockersamples/voting_app', './')
  }
  stage('Push voting app image') {
    docker.withRegistry('https://dell-harbor.dell.ecore.af.smil.mil', 'harbor-credentials') {
    img.push()
    }
  }
}