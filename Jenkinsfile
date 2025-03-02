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

  post {
    always {
      telegramSend("Build #${env.BUILD_NUMBER} started for ${env.JOB_NAME}")
    }
    success {
      telegramSend("Build #${env.BUILD_NUMBER} succeeded for ${env.JOB_NAME}: ${env.BUILD_URL}")
      echo 'Success.'
    }
    failure {
      telegramSend("Build #${env.BUILD_NUMBER} failed for ${env.JOB_NAME}: ${env.BUILD_URL}")
      echo 'Failure.'
    }
  }
}
