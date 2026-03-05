pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'echo "Building…" && mkdir -p build && echo "hello" > build/app.bin'
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }

  post {
    success {
      // Run just this part on your local machine agent
      node('Demo-Jenkins') {
        unstashOrCopy()
      }
    }
  }
}

// Shared method if you want: copy from Jenkins master to your local agent
def unstashOrCopy() {
  // Option 1: re-use artifacts by copying from master using 'copyArtifacts' if this is a multibranch / external job
  // library step alternative if same pipeline: use 'dir' + 'download'—but easiest is re-run step on agent or fetch via curl
  step([$class: 'CopyArtifact',
        projectName: env.JOB_NAME,
        selector: [$class: 'SpecificBuildSelector', buildNumber: env.BUILD_NUMBER],
        filter: 'build/**', fingerprintArtifacts: true])

  sh 'mkdir -p /path/to/my/local/drop && cp -r build/* /path/to/my/local/drop/'
}