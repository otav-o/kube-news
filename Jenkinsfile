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
  }
}