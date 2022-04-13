pipeline {
  agent any

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample',
        fingerprintArtifacts: true,
        projectName: 'devops',
        selector: lastSuccessful()
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-pkey", keyFileVariable: 'keyfile')]) {
            sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./devops vagrant@10.10.50.3:'
        }
      }
    }

  }
}