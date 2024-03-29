pipeline {

  environment {
    dockerimagename = "siddharthwalke01/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git credentialsId: 'GitHub-Sid', url: 'https://github.com/siddharthwalke/jenkins-k8s-deployment.git'
  }
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'docker-sid'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          // Update the configuration file names
          def deploymentConfigs = load "deployment.yaml"
          def serviceConfigs = load "service.yaml"
          
          kubernetesDeploy(configs: deploymentConfigs + serviceConfigs)
        }
      }
    }
}
