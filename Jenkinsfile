pipeline {

  environment {
    dockerimagename = "waiyansoe/nodeapp:latest"
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

    stage('Pushing Image to Docker Hub') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Minikube') {
      steps {
        // container('kubectl'){
        //   withCredentials([kubeconfigFile(credentialsId: 'minikube')]) {
        //     sh 'kubectl apply -f deploymentservice.yml --kubeconfig=/root/.kube/config'
        //   }
        // }
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }
  }

}