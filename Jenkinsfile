pipeline {
  environment {
    dockerimagename = "reg.minikube.local/kdkp/wy-cicd-1:latest"
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

    stage('Deploying App to Minikube') {
      steps {
        script {
          // Run the Docker container in Minikube
          sh "docker run -d --name wy-cicd-1 -p 4000:4000 ${dockerimagename}"
        }
      }
    }
  }
}
