pipeline {
  agent any
  stages {
    stage('BuildCapture') {
      steps {
        echo 'Capturing build using cov-build'
        sh '''echo ${CHANGE_SET}
/opt/coverity/cov-build --desktop --dir idir --fs-capture-list ${CHANGE_SET} --no-command '''
      }
    }
  }
}