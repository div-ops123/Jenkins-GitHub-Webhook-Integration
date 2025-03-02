pipeline {
  agent any

  environment {
    // Telegram configuration
    TOKEN = credentials('telegram-credentials')
    CHAT_ID = credentials('telegram-chatid')
  }

  stages {
    stage('Test Telegram Send') {
      steps {
         script {
            sh "curl -X POST -H 'Content-Type: application/json' -d '{\"chat_id\":${CHAT_ID}, \"text\": \"Pipeline started!\", \"disable_notification\": false}' https://api.telegram.org/bot${TOKEN}/sendMessage"
         }
      }
    }
    
//    stage('Start') { 
//       steps {
//         telegramSend("Build #${env.BUILD_NUMBER} started for ${env.JOB_NAME}")
//       }
//    }

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
    success {
//      telegramSend("Build #${env.BUILD_NUMBER} succeeded for ${env.JOB_NAME}: ${env.BUILD_URL}")
       sh "curl -X POST -H 'Content-Type: application/json' -d '{\"chat_id\":${CHAT_ID}, \"text\": \"Pipeline succeeded!\", \"disable_notification\": false}' https://api.telegram.org/bot${TOKEN}/sendMessage"
       echo 'Success.'
    }
    failure {
//      telegramSend("Build #${env.BUILD_NUMBER} failed for ${env.JOB_NAME}: ${env.BUILD_URL}")
       sh "curl -X POST -H 'Content-Type: application/json' -d '{\"chat_id\":${CHAT_ID}, \"text\": \"Pipeline failed!\", \"disable_notification\": false}' https://api.telegram.org/bot${TOKEN}/sendMessage"
       echo 'Failure.'
    }
  }
}
