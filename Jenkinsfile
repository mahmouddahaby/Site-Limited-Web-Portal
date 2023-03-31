pipeline {
  agent {
      label "agent"
  }
  
  triggers {
      pollSCM '*/5 * * * *' // Poll SCM every 5 minutes
      branches('master') // Only run when updates happen on the master branch
  }

  stages {
    stage('Build Docker image') {
      steps {
        script {
          def tag = "build-${env.BUILD_NUMBER}"
          sh "docker build -t FS-app:${tag} ."
          sh "docker push 328440967566.dkr.ecr.us-east-1.amazonaws.com/FS-app:${tag}"
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          sh "kubectl apply -f app-k8s/deployment.yaml"
          sh "kubectl apply -f app-k8s/service.yaml"
          sh "kubectl apply -f app-k8s/fs-app-policy.yaml"
        }
      }
    }
  }
}

