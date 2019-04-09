node {
  withEnv(['DOCKER_CONTENT_TRUST=0',
  'DOCKER_CONTENT_TRUST_SERVER=${content-trust-server-url}',
  'DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE=${docker-content-trust-root-passphrase}',
  'DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE=${docker-content-trust-repo-passphrase}']){
    def img
    stage('Clone repository') {
      checkout scm
    }
    stage('Build image') {
      img = docker.build('sdtf/vote_app:latest', '.')
    }
    withEnv(['DOCKER_CONTENT_TRUST=1']){
    stage('Push image') {
      sh 'docker tag sdtf/vote_app:latest dell-harbor.dell.ecore.af.smil.mil/sdtf/vote_app:latest'
      sh 'docker login dell-harbor.dell.ecore.af.smil.mil'
      sh 'docker push dell-harbor.dell.ecore.af.smil.mil/sdtf/vote_app:latest'
      }
    }
  }
}