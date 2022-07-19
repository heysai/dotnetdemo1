pipeline {
  environment {
    imagename = "bermtechacr.azurecr.io/bermtechacr"
    registryCredential = 'acr-credential'
    dockerImage = ''
    registryURl = "bermtechacr.azurecr.io"
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/MaroofaTanweer/dotnetdemo', branch: 'CICD', credentialsId: 'b35c3051-0f6a-48ae-9f75-712f018f59f0'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry("http://${registryURl}", registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"
      }
    }
  }
}