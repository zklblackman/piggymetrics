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
          sh './mvnw package -Dmaven.test.skip=true'
          sh 'docker build -t zhangkanglong/demo:account-service ./account-service/'
          sh 'docker login --username=zhangkanglong --password=zkl.zhang.1994'
          sh 'docker push zhangkanglong/demo:account-service'
      }
    }

  }
}
