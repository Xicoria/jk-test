pipeline {
  agent {
    kubernetes {
      label 'mypod'
      containerTemplate {
        name 'php'
        image 'php'
        ttyEnabled true
        command 'cat'
      }
    }
  }
  stages {
    stage('Run php version') {
      steps {
        container('php') {
          sh 'mvn -version'
        }
      }
    }
  }
}
