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
            sh 'docker build -t zhangkanglong/demo:auth-service ./auth-service/'
            sh 'docker build -t zhangkanglong/demo:config ./config/'
            sh 'docker build -t zhangkanglong/demo:gateway ./gateway/'
            sh 'docker build -t zhangkanglong/demo:monitoring ./monitoring/'
            sh 'docker build -t zhangkanglong/demo:notification-service ./notification-service/'
            sh 'docker build -t zhangkanglong/demo:registry ./registry/'
            sh 'docker build -t zhangkanglong/demo:statistics-service ./statistics-service/'
            sh 'docker build -t zhangkanglong/demo:turbine-stream-service ./turbine-stream-service/'

            sh 'docker login --username=zhangkanglong --password=$DOCKER_PASSWORD'

            sh 'docker push zhangkanglong/demo:account-service'
            sh 'docker push zhangkanglong/demo:auth-service'
            sh 'docker push zhangkanglong/demo:config'
            sh 'docker push zhangkanglong/demo:gateway'
            sh 'docker push zhangkanglong/demo:monitoring'
            sh 'docker push zhangkanglong/demo:notification-service'
            sh 'docker push zhangkanglong/demo:registry'
            sh 'docker push zhangkanglong/demo:statistics-service'
            sh 'docker push zhangkanglong/demo:turbine-stream-service'
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