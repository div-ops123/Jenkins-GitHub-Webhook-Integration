pipeline {
  agent any

  stages {
    stage('Pre-Build') {
      steps {
        echo 'Pre Build...'
        echo 'Sending status pre-build to Mail, telegram, slack, ...'
      }
    }

    stage('Build') {
      steps {
        echo 'Building...'
        echo 'Running Docker build...'
      }
    }

    stage('Test') {
      steps {
        echo 'Testing...'
      }
    }

    stage('Push') {
      steps {
        echo 'Pushing...'
        echo 'Running Docker build...'
      }
    }    
  }
}
