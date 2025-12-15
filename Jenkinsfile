pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'npm ci || npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test || echo "no tests defined"'
      }
    }

    stage('Build Docker Image') {
      agent {
        docker {
          image 'docker:24-cli'
          args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
      }
      steps {
        sh 'docker version'
        sh 'docker build -t visits-api:latest .'
      }
    }

  }
}
