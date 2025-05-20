pipeline {
  agent any
  
  environment {
    DOCKER_IMAGE = 'sadamquispe/pageducacion'
    DOCKER_TAG = 'latest'
  }
  
  stages {
    stage('Deploy pageducacion') {
      steps {
        withCredentials(bindings: [
          string(credentialsId: 'kubernete-jenkis-server-account', variable: 'api_token')
        ]) {
          sh 'kubectl --token $api_token --server https://192.168.49.2:8443 --insecure-skip-tls-verify=true apply -f deployment-pageducacion.yaml'
        }
      }
    }
  }
  
  post {
    always {
      echo 'Pipeline completado'
    }
  }
}