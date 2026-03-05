pipeline {
  agent any  // runs on the built-in node (controller)

  environment {
    // Change to where you want the file(s) to appear on this machine
    TARGET_DIR = '/home/aditya/downloads/builds'
  }

  options {
    // Keep logs cleaner; optional
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
          # Your real build command(s) go here
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
      // Copy artifacts to your local drop folder on the same machine
      sh '''
        set -e
        mkdir -p "${TARGET_DIR}"
        cp -r build/* "${TARGET_DIR}/"
      '''
      echo "Artifacts copied to ${env.TARGET_DIR}"
    }
  }
}
