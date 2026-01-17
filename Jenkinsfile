pipeline {
  agent any

  environment {
    IMAGE = "pranavadmin/devsecops-demo"
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/pranav-admin/devsecops-demo.git'
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t $IMAGE:latest .'
      }
    }

    stage('Security Scan') {
      steps {
        sh 'docker scan $IMAGE:latest || true'
      }
    }

stage('Push Image') {
  steps {
    withCredentials([usernamePassword(
      credentialsId: 'dockerhub-creds',
      usernameVariable: 'DOCKER_USER',
      passwordVariable: 'DOCKER_PASS'
    )]) {
      sh '''
        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
        docker push pranavadmin/devsecops-demo:latest
      '''
    }
  }
}

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }
  }
}


