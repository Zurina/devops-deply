pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'prod'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
    string(name: 'branch',
          defaultValue: 'main',
          description: 'branch to copy artifact from'
    )
  }

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'devops',
        fingerprintArtifacts: true,
        projectName: 'devops-test-project/${params.branch}}',
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