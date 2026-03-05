pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Running build step..."'
            }
        }

        stage('Tests') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('Package') {
            steps {
                sh 'echo "Packaging build..."'
                sh 'mkdir -p artifacts'
                sh 'echo "Build output" > artifacts/output.txt'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'artifacts/**/*', fingerprint: true
            }
        }
    }
}
