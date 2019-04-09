node {
  withEnv(["DOCKER_CONTENT_TRUST=0",
  "DOCKER_CONTENT_TRUST_SERVER=${ContentTrustServerURL}",
  "DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE=${ContentTrustRootPassphrase}",
  "DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE=${ContentTrustRepoPassphrase}"]){
    def image
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
    stage('Set k8s image') {
    withKubeConfig([credentialsId: 'voting-app',
                serverUrl: 'https://voting-app:8443',
                contextName: 'default',
                clusterName: 'default'
                ]) {
        sh "kubectl set image deployment voting-app voting-app=dell-harbor.dell.ecore.af.smil.mil/sdtf/vote_app:latest -n VoteApp --record=true"
    }
    }
  }
}