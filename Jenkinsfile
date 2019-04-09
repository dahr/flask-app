node {
  withEnv(["DOCKER_CONTENT_TRUST=0",
  "DOCKER_CONTENT_TRUST_SERVER=${ContentTrustServerURL}",
  "DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE=${ContentTrustRootPassphrase}",
  "DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE=${ContentTrustRepoPassphrase}"]){
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
    withCredentials([usernameColonPassword(credentialsId: 'pksAccess', variable: 'USERPASS')]){
    withEnv(["PKS_USER_PASSWORD=$USERPASS"]){
    stage('Set k8s image') {
      sh "pks login -a api.pks.dell.ecore.af.smil.mil -u dahr -k -p $USERPASS"
      def creds = sh 'pks get-credentials VoteApp'
      echo "$creds"
      }
    }
  }
}
}