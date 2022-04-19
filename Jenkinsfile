pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'prod'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
  }

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'devops',
        fingerprintArtifacts: true,
        projectName: 'devops-test-project/mathias',
        selector: lastSuccessful()
      }
    }
    stage('Deploy With Ansible') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-pkey", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i ${DEPLOY_TO}.ini playbook.yml'
        }
      }
    }
  }
}