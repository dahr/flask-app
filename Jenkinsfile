node {
    def img
    stage('Clone Repository') {
      checkout scm
    }
    stage('Build, Push & Sign Image') {
      docker.withRegistry('https://harbor.corp.local', 'harbor-creds') {

        /* Build Image */
        def customImage = docker.build("demo/demo-app:$BUILD_ID", '.')

        /* Push the container to the custom Registry */
        customImage.push()

        /* Scan the image immediately */
        sh "curl -H 'Accept: application/json, text/plain' -H 'Content-Type: application/json' -H 'Authorization: Basic YWRtaW46Vk13YXJlMSE=' -k -X POST 'https://harbor.corp.local/api/repositories/demo/demo-app/tags/$BUILD_ID/scan'"
      }
    }
    stage('Set k8s Image') {
    withKubeConfig([credentialsId: 'demo-app',
                serverUrl: 'https://my-cluster.corp.local:8443',
                contextName: 'my-cluster',
                clusterName: 'my-cluster'
                ]) {

        /* Deploy update image */
        sh "kubectl set image deployment demo-app demo-app=harbor.corp.local/demo/demo-app:$BUILD_ID -n demo --record=true"
    }
  }
}