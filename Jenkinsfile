pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('d268aa49-b7cd-475c-b31e-b41be3d36f31')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t draxter67/django-project:1.2 .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push draxter67/django-project:1.2'
      }
    }
    stage('Deploy on K8s') {
      steps { 
        sh 'kubectl apply -f K8s/.'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}