pipeline {
  environment {
    dockerImageName = "reg.minikube.local/kdkp/wy-cicd-2:3"
    registryCredential = 'harborlogin'
    kubeConfig = ' ~/.kube/config'
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        checkout scm
      }
    }

    stage('Build and Push Docker Image') {
      steps {
        script {
          // Build Docker image
          dockerImage = docker.build dockerImageName

          // Push the Docker image to Harbor
          docker.withRegistry('http://reg.minikube.local', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App to Minikube') {
      steps {
        script {
          // Configure kubectl with the provided kubeconfig
          withKubeConfig([credentialsId: ' kubeConfig', ' ~/.kube/config']) {
            // Apply the Kubernetes configuration
            sh "kubectl apply -f deploymentservice.yml"
          }
        }
      }
    }
  }
}
