pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "jx-staging"
    PROXY = "http_proxy=http://192.168.1.127:8118"
    NO_PROXY = "no_proxy=localhost,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,127.0.0.1,*.afeib.cn,*.datangliang.com,jenkins-x-chartmuseum"
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh 'export $PROXY && export $NO_PROXY && jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'export $PROXY && export $NO_PROXY && jx step helm apply'
          }
        }
      }
    }
  }
}
