pipeline {
  agent any  // Linux/WSL controller

  environment {
    // WSL path that maps to C:\Users\aditya.routh\Downloads\builds
    TARGET_DIR = '/mnt/c/Users/aditya.routh/Downloads/builds'
  }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Prepare Deliverables') {
      steps {
        // Create a clean staging folder and copy repo into it (excluding git + temp stuff)
        sh '''#!/usr/bin/env bash
set -eux

# Clean and create staging area
rm -rf package
mkdir -p package/repo

# Copy everything except VCS and typical transient dirs/files
# Use rsync if available (fast, preserves structure), otherwise fall back to find/cp
if command -v rsync >/dev/null 2>&1; then
  rsync -a \
    --exclude='.git/' \
    --exclude='.gitmodules' \
    --exclude='.github/' \
    --exclude='.venv/' \
    --exclude='venv/' \
    --exclude='node_modules/' \
    --exclude='__pycache__/' \
    --exclude='*.pyc' \
    --exclude='*.pyo' \
    --exclude='.mypy_cache/' \
    --exclude='.pytest_cache/' \
    --exclude='Jenkinsfile' \
    ./ package/repo/
else
  # Fallback copy using tar to preserve structure (without compression)
  tar --exclude-vcs \
      --exclude='.venv' \
      --exclude='venv' \
      --exclude='node_modules' \
      --exclude='__pycache__' \
      --exclude='*.pyc' \
      --exclude='*.pyo' \
      --exclude='.mypy_cache' \
      --exclude='.pytest_cache' \
      --exclude='Jenkinsfile' \
      -cf - . | ( cd package/repo && tar -xf - )
fi

echo "=== Staged content ==="
ls -la package/repo | sed -n '1,200p'
'''
      }
    }

    stage('Archive in Jenkins (for history/downloads)') {
      steps {
        archiveArtifacts artifacts: 'package/repo/**', fingerprint: true
      }
    }
  }

  post {
    success {
      // Copy staged repo to your Windows folder via WSL mount
      sh '''#!/usr/bin/env bash
set -eux

mkdir -p "${TARGET_DIR}"

# Copy contents (not the top-level 'repo' folder name)
cp -vr package/repo/* "${TARGET_DIR}/" || true

echo "=== Target listing after copy ==="
ls -la "${TARGET_DIR}" | sed -n '1,200p'
'''
      echo "All repository files delivered to ${env.TARGET_DIR}"
    }
  }
}
``
