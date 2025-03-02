pipeline {
  agent any

  environment {
    // Telegram configuration
    TOKEN = credentials('telegram-credentials')
    CHAT_ID = credentials('telegram-chatid')
    
    // Telegram Message Success and Failure
    TEXT_SUCCESS_BUILD = "'${JOB_NAME}' Build Job Successful."
    TEXT_FAILURE_BUILD = "'${JOB_NAME}' Build Job Failed."
  }


  stages {

    stage('Pre-Build') {
      steps {
        script {
           // Telegram message pre-build
           env.CURRENT_BUILD_NUMBER = "${currentBuild.number}"
           env.GIT_MESSAGE = sh(returnStdout: true, script: "git log -n 1 --format=%s ${GIT_COMMIT}").trim()
           env.GIT_AUTHOR = sh(returnStdout: true, script: "git log -n 1 --format=%ae ${GIT_COMMIT}").trim()
           env.GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
           env.GIT_INFO = "Branch(Version): ${GIT_BRANCH}\nLast Message: ${GIT_MESSAGE}\nAuthor: ${GIT_AUTHOR}\nCommit: ${GIT_COMMIT_SHORT}"
           env.TEXT_BREAK = "--------------------------------------------------------------"
           env.TEXT_PRE_BUILD = "${TEXT_BREAK}\n${GIT_INFO}\n${JOB_NAME}\nBuild #${CURRENT_BUILD_NUMBER} is Building..."
        }

        sh "curl --location --request POST 'https://api.telegram.org/bot${TOKEN}/sendMessage' --form text='${TEXT_PRE_BUILD}' --form chat_id='${CHAT_ID}'"
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
       sh "curl --location --request POST 'https://api.telegram.org/bot${TOKEN}/sendMessage' --form text='${TEXT_SUCCESS_BUILD}' --form chat_id='${CHAT_ID}'"
    }

    failure {
       sh "curl --location --request POST 'https://api.telegram.org/bot${TOKEN}/sendMessage' --form text='${TEXT_FAILURE_BUILD}' --form chat_id='${CHAT_ID}'"
    }
  }
}
