pipeline {
  environment {
    dockerimagename = "reg.minikube.local/kdkp/wy-cicd-2:3"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        // No need for explicit Git credentials for public repositories
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          // Build Docker image
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image to Harbor') {
      environment {
        registryCredential = 'harborlogin'
      }
      steps {
        script {
          // Push the Docker image to Harbor
          docker.withRegistry('http://reg.minikube.local', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploying App on Docker') {
      steps {
        script {
          // Run the Docker container in Minikube
          sh "kubectl apply -f deploymentservice.yml"
        }
      }
    }
  }
}
