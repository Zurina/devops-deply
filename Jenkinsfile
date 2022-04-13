pipeline {
  agent any

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true, projectName: 'sample', selector: lastSuccessful()
      }
    }
    stage('Deploy') {
      steps {
        sh 'scp ./sample vagrant@10.10.50.3:'
      }
    }

  }
}