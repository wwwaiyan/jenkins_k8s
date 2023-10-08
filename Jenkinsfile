pipeline {

  environment {
    dockerimagename = "homelab/nodeapp:latest"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/w4an/jenkins_k8s.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image to Harbor') {
      environment {
               registryCredential = 'harborlogin'
           }
      steps{
        script {
          docker.withRegistry( 'http://reg.minikube.local', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Minikube') {
      steps {
        container('kubectl'){
          withCredentials([kubeconfigFile(credentialsId: 'minikube')]) {
            sh 'kubectl apply -f deploymentservice.yml'
          }
        }
        // script {
        //   kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        // }
      }
    }
  }

}