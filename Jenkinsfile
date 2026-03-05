pipeline {
  agent any  // runs on the built-in node (controller)

  environment {
    // Change to your desired local path
    TARGET_DIR = 'C:\\Users\\aditya\\Downloads\\builds'
  }

  options {
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        bat '''
          if exist build rmdir /s /q build
          mkdir build
          rem Your real build commands here
          echo hello> build\\app.bin
        '''
      }
    }

    stage('Archive (optional but recommended)') {
      steps {
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }

  post {
    success {
      bat '''
        if not exist "%TARGET_DIR%" mkdir "%TARGET_DIR%"
        xcopy /y /e /i build "%TARGET_DIR%\\"
      '''
      echo "Artifacts copied to ${env.TARGET_DIR}"
    }
  }
}
