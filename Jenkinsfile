node {
  /*withEnv(["DOCKER_CONTENT_TRUST=0",
  "DOCKER_CONTENT_TRUST_SERVER=${ContentTrustServerURL}",
  "DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE=${ContentTrustRootPassphrase}",
  "DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE=${ContentTrustRepoPassphrase}"]){*/
    def image
    stage('Clone repository') {
      checkout scm
    }
    stage('Build image') {
      img = docker.build("demo/demo-app:$BUILD_ID", '.')
    }
    /*withEnv(['DOCKER_CONTENT_TRUST=1']){*/
      stage('Push image') {
        sh "docker tag demo/demo-app:$BUILD_ID harbor.corp.local/demo/demo-app:$BUILD_ID"
        sh 'docker login harbor.corp.local'
        sh "docker push harbor.corp.local/demo/demo-app:$BUILD_ID"
        //sh "curl -H 'Accept: application/json, text/plain' -H 'Content-Type: application/json' -H 'Authorization: Basic YWRtaW46Vk13YXJlMSE=' -X POST 'https://dell-harbor.dell.ecore.af.smil.mil/api/repositories/sdtf/vote_app/tags/$BUILD_ID/scan'"
        }
    stage('Set k8s image') {
    withKubeConfig([credentialsId: 'demo-app',
                serverUrl: 'https://my-cluster.corp.local:8443',
                contextName: 'my-cluster',
                clusterName: 'my-cluster'
                ]) {
        sh "kubectl set image deployment demo-app demo-app=harbor.corp.local/demo/demo-app:$BUILD_ID -n default --record=true"
    }
  }
}