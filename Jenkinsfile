pipeline {
  agent any  // runs on the built-in node (Linux/WSL controller)

  environment {
    // WSL path to your Windows folder
    TARGET_DIR = '/mnt/c/Users/aditya.routh/Downloads/builds'
  }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build') {
      steps {
        sh '''
          set -eux
          rm -rf build
          mkdir -p build

          # TODO: Replace this placeholder with your real build commands
          # Example (Node): npm ci && npm run build
          # Example (Maven): mvn -B -DskipTests clean package
          # Example (Gradle): ./gradlew clean build -x test
          echo "hello from $(hostname) at $(date)" > build/app.bin

          echo "=== Build output (build/) ==="
          ls -la build
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
      sh '''
        set -eux
        echo "=== Ensure target dir exists ==="
        mkdir -p "${TARGET_DIR}"
        ls -ld "${TARGET_DIR}"

        echo "=== Copy artifacts to target ==="
        cp -vr build/* "${TARGET_DIR}/" || true

        echo "=== Target listing after copy ==="
        ls -la "${TARGET_DIR}"
      '''
      echo "Artifacts copied to ${env.TARGET_DIR}"
    }
  }
}
