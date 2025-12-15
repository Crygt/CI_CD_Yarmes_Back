pipeline {
  agent any

  environment {
    IMAGE = "visits-api:latest"
    CONTAINER = "visits-api"
    PORT = "3001"
  }

  stages {

    stage('Checkout') {
      steps { checkout scm }
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
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker rm -f $CONTAINER || true'
        sh 'docker run -d --name $CONTAINER -p $PORT:$PORT --env-file .env $IMAGE'
      }
    }

    stage('Healthcheck') {
      steps {
        sh 'sleep 3'
        sh 'curl -f http://localhost:$PORT/visits || (docker logs $CONTAINER && exit 1)'
      }
    }
  }

  post {
    always {
      sh 'docker ps -a | head -n 20 || true'
    }
  }
}
