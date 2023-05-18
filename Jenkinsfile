pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker')
    
  }
  stages {

    stage('clone') {
      steps {
        sh 'echo ${BUILD_ID}'
        sh 'echo ${NODE_NAME}'
        git branch: 'main', credentialsId: 'test1', url: 'https://github.com/maheswarsarma/DockerDemo.git'
        sh 'docker build -t maheswarsarma/jenkins-docker-hub-${NODE_NAME}-${BUILD_ID} . '
      }
    }
    
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker image push docker.io/maheswarsarma/jenkins-docker-hub-${NODE_NAME}-${BUILD_ID}'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
