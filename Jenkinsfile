pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "jx-staging"
    CHART_REPOSITORY = "http://chartmuseum.jx.afeib.cn"
    http_proxy = "http://192.168.1.127:8118"
    no_proxy = "localhost,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,127.0.0.1,*.afeib.cn,*.datangliang.com,jenkins-x-chartmuseum"
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh 'curl -I www.google.com'
            sh 'helm init --client-only --stable-repo-url=https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts'
            //sh 'export $PROXY && export $NO_PROXY && jx step helm build'
            sh 'jx step helm build'
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
            sh 'helm init --client-only --stable-repo-url=https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts'
            //sh 'export $PROXY && export $NO_PROXY &&  jx step helm apply'
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
}
