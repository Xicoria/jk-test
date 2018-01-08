pipeline {
  agent {
    kubernetes {
      label 'test-pod'
      containerTemplate {
        name 'php'
        image 'php'
        ttyEnabled true
      }
    }
  }
  stages {
    stage('Run php version') {
      steps {
        container('php') {
          sh 'php -v'
        }
      }
    }
  }
}
