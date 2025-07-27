pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/orodriguez40/final-devops-pipeline.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Unit Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Performance Test') {
      steps {
        // using official jMeter Docker image
        sh 'docker run --rm -v ${WORKSPACE}/jmeter:/jmeter apache/jmeter:5.4.1 -n -t /jmeter/test-plan.jmx -l /jmeter/results.jtl'
      }
      post {
        always {
          archiveArtifacts artifacts: 'jmeter/results.jtl', fingerprint: true
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t demo-app:latest .'
      }
    }
    stage('Deploy') {
      steps {
        sh 'docker-compose up -d --build'
      }
    }
  }
}
