pipeline {
  agent any
  stages {
    stage('BuildCapture') {
      steps {
        echo 'Capturing build using cov-build'
        sh '''git-diff HEAD HEAD-1 > file_list.txt
/opt/coverity/cov-build --desktop --dir idir --fs-capture-list file_list.txt --no-command '''
      }
    }
  }
}