node {
    def img
    stage('Clone repository') {
      checkout scm
    }
    stage('Build & push image') {
      docker.withRegistry('https://harbor.corp.local', 'harbor-creds') {

      def customImage = docker.build("demo/demo-app:$BUILD_ID", '.')

      /* Push the container to the custom Registry */
      customImage.push()
      }
    }
    stage('Set k8s image') {
    withKubeConfig([credentialsId: 'demo-app',
                serverUrl: 'https://my-cluster.corp.local:8443',
                contextName: 'my-cluster',
                clusterName: 'my-cluster',
                namespace:  "demo"
                ]) {
        sh "kubectl set image deployment demo-app demo-app=harbor.corp.local/demo/demo-app:$BUILD_ID -n demo --record=true"
        sh "curl -H 'Accept: application/json, text/plain' -H 'Content-Type: application/json' -H 'Authorization: Basic YWRtaW46Vk13YXJlMSE=' -X POST 'https://harbor.corp.local/api/repositories/demo/demo-app/tags/$BUILD_ID/scan'"
    }
  }
}