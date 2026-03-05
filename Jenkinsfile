pipeline {
  agent any  // runs on the built-in node (Linux controller)

  environment {
    // Change this to the path on your Linux box where you want files copied
    TARGET_DIR = 'C:\Users\aditya.routh\Downloads\builds'
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
        sh '''
          set -e
          rm -rf build
          mkdir -p build
          # Replace this with your actual build command(s)
          echo "hello" > build/app.bin
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
      // Copy to your local drop folder on the same machine
      sh '''
        set -e
        mkdir -p "${TARGET_DIR}"
        cp -r build/* "${TARGET_DIR}/"
      '''
      echo "Artifacts copied to ${env.TARGET_DIR}"
    }
  }
}

