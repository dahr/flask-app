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
    withKubeConfig([credentialsId: 'demo',
                serverUrl: 'https://my-cluster.corp.local:8443',
                contextName: 'my-cluster',
                clusterName: 'my-cluster'
                namespace:  "demo"
                ]) {
        sh "kubectl set image deployment demo-app demo-app=harbor.corp.local/demo/demo-app:$BUILD_ID -n default --record=true"
    }
  }
}