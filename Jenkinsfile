pipeline {
  agent {
    docker {
      image 'maven:latest'
      args '-v /opt/coverity/coverity_static_analysis:/opt/coverity/coverity_static_analysis --hostname covuser-vm --network host'
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
/opt/coverity/coverity_static_analysis/bin/cov-build --desktop --dir idir --fs-capture-list file_list.txt --no-command 
/opt/coverity/coverity_static_analysis/bin/cov-run-desktop --dir idir --host $COVERITY_HOST --port $COVERITY_PORT --stream $COVERITY_STREAM --reference-snapshot $COVERITY_SNAPSHOT @@file_list.txt'''
      }
    }
  }
  environment {
    COVERITY_HOST = '192.168.99.1'
    COVERITY_PORT = '8443'
    COVERITY_STREAM = 'webgoat8'
    COVERITY_SNAPSHOT = 'latest'
  }
}