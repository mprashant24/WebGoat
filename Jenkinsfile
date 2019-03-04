pipeline {
  agent {
    docker {
      image 'maven:latest'
      args '-v /opt/coverity/coverity_static_analysis:/opt/coverity/coverity_static_analysis --hostname covuser-vm --mac-address 08:00:27:60:53:AB'
    }

  }
  stages {
    stage('BuildCapture') {
      steps {
        echo 'Capturing build using cov-build'
        sh '''git show
git diff --name-only HEAD^ HEAD > file_list.txt
cat file_list.txt
ls -lrt /opt/coverity/coverity_static_analysis
/opt/coverity/coverity_static_analysis/cov-build --desktop --dir idir --fs-capture-list file_list.txt --no-command '''
      }
    }
  }
}