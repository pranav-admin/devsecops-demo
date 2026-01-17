pipeline {
  agent any

  environment {
    IMAGE = "pranav-admin/devsecops-demo"
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
        sh '''
        docker push $IMAGE:latest
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }
  }
}


