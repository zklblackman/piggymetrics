pipeline {
  agent none
	triggers {
    pollSCM 'H/5 * * * *'
  }
  stages {
    stage('Build Image & Upload') {
      when {
        branch "master"
      }
      agent {
        label 'docker'
      }
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
              sh 'docker build -t zhangkanglong/demo:account-service ./account-service/'
              sh 'docker login --username=$DOCKER_USER --password=$DOCKER_PASSWORD'
              sh 'docker push zhangkanglong/demo:account-service'
              }
          }
    }
    stage('deploy') {
      when {
        branch "master"
      }
      agent {
        label 'docker'
      }
      steps {
        withKubeConfig([
          credentialsId: 'k8s',
          contextName: 'kubernetes'
        ]) {
          sh 'kubectl apply -f ./account-service/account-service.yaml'
        }
      }
    }
  }
}