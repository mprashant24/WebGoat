pipeline {
  agent {
    docker {
      image 'maven:latest'
      args '-v /opt/coverity/coverity_static_analysis:/opt/coverity/coverity_static_analysis --hostname covuser-vm --network host'
    }

  }
  stages {
    stage('BuildCapture') {
      parallel {
        stage('Full build capture') {
          steps {
            sh '''git ls-files > capture-files.txt
/opt/coverity/coverity_static_analysis/bin/cov-build --dir idir-full --fs-capture-list capture-files.txt --no-command'''
          }
        }
        stage('Incremental Scan') {
          steps {
            sh '''git diff --name-only HEAD^ HEAD > file_list.txt
cat file_list.txt
/opt/coverity/coverity_static_analysis/bin/cov-build --desktop --dir idir --fs-capture-list file_list.txt --no-command 
'''
            sh '/opt/coverity/coverity_static_analysis/bin/cov-run-desktop --dir idir --host $COVERITY_HOST --port $COVERITY_PORT --stream $COVERITY_STREAM --reference-snapshot $COVERITY_SNAPSHOT --auth-key-file /opt/coverity/coverity_static_analysis/bin/auth-key-file @@file_list.txt'
          }
        }
      }
    }
    stage('Full Analysis') {
      steps {
        sh '/opt/coverity/coverity_static_analysis/bin/cov-analyze --dir idir-full --all --webapp-security '
      }
    }
    stage('Full Commit') {
      steps {
        sh '/opt/coverity/coverity_static_analysis/bin/cov-commit-defects --dir idir-full --host $COVERITY_HOST --https-port $COVERITY_PORT --stream $COVERITY_STREAM --auth-key-file /opt/coverity/coverity_static_analysis/bin/auth-key-file '
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