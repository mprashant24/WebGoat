pipeline {
  agent {
    docker {
      args '-v /opt/coverity/coverity_static_analysis:/opt/coverity/coverity_static_analysis --hostname covuser-vm --network host -u root:root'
      image 'maven:latest'
      label 'Docker'
    }

  }
  stages {
    stage('Desktop build capture') {
      steps {
        sh '''git diff --name-only $GIT_COMMIT $GIT_PREVIOUS_SUCCESSFUL_COMMIT > change_list.txt
cat change_list.txt
echo webgoat-lessons/sql-injection/src/main/java/org/owasp/webgoat/plugin/advanced/SqlInjectionChallenge.java > change_list.txt
/opt/coverity/coverity_static_analysis/bin/cov-build --desktop --dir idir-desktop --fs-capture-list change_list.txt --no-command'''
      }
    }
    stage('Incremental Analysis') {
      steps {
        sh '/opt/coverity/coverity_static_analysis/bin/cov-run-desktop --dir idir-desktop --host $COVERITY_HOST --port $COVERITY_PORT --stream $COVERITY_STREAM --reference-snapshot $COVERITY_SNAPSHOT --auth-key-file /opt/coverity/coverity_static_analysis/bin/auth-key-file --ignore-uncapturable-inputs true --text-output analyze_result.txt --text-output-style multiline --json-output-v6 analyze_result.json --present-in-reference false @@change_list.txt'
      }
    }
    stage('Clean up') {
      steps {
        cleanWs(cleanWhenSuccess: true)
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
