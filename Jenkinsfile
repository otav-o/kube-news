pipeline {
  agent any

  stages {
    stage ("Build Docker Image") {
      steps {
        script {
          dockerapp = docker.build("otaviodioscanio/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
        }
      }
    }
    stage ("Push to DockerHub") {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-login-otavio') {
            dockerapp.push('latest')
            dockerapp.push("${env.BUILD_ID}")
          }
        }
      }
    }
    stage ("Deploy Kubernetes") {
      environment {
        tag_version = "${env.BUILD_ID}"
      }
      steps {
        withKubeConfig([credentialsId: 'kubeconfig-file-digital-ocean']) {
          sh 'sed -i "s/{{TAG}}/$tag_version/g" ./src/deployment.yml'
          sh 'kubectl apply -f ./src/deployment.yml' 
        }
      }
    }
  }
}