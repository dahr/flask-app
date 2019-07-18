node {
    def img
    stage('Clone repository') {
      sh "echo cloning"
      checkout scm
    }
    stage('Build image') {
      img = docker.build("demo/demo-app:$BUILD_ID", '.')
    }
    stage('Push image') {
      sh "docker tag demo/demo-app:$BUILD_ID harbor.corp.local/demo/demo-app:$BUILD_ID"
      sh 'docker login harbor.corp.local'
      sh "docker push harbor.corp.local/demo/demo-app:$BUILD_ID"
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