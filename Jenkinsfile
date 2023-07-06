pipeline {

  environment {
        registry = "8950539357/j2k:1.0.0"
        registryCredential = 'AbhiDockerHubCreds' 
  }

  stages {
    stage('Checkout Source') {
      agent any
      steps {
        git branch: 'main', url: 'https://github.com/AbhiGowdaIndia/J2K.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'DockerHubCreds'
           }
      steps{
        script {
          docker.withRegistry( 'https://github.com/AbhiGowdaIndia/J2K.git', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      agent {
        node{
          label 'k8s'
        }
      }
      steps {
        script {
          sh "kubectl apply -f ./deployment.yaml -n qp"
          sh "kubectl apply -f ./service.yaml -n qp"
        }
      }
    }

  }
}
