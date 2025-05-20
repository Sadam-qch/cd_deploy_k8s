pipeline {
  agent {
    docker {
      image 'node:21-slim'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  
  environment {
    DOCKER_IMAGE = 'sadamquispe/pageducacion'
    DOCKER_TAG = 'latest'
  }
  
  stages {
    stage('Preparar Entorno') {
      steps {
        sh '''apt-get update && apt-get install -y curl git
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
mv kubectl /usr/local/bin/
node --version
npm --version
git --version'''
      }
    }
    
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