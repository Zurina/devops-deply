pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'prod'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
    string(name: 'branch', defaultValue: 'main', description: 'branch to copy artifact from')
    string(name: 'host_ip', defaultValue: '', description: 'Host IP to validate deployment')
  }

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'devops',
        fingerprintArtifacts: true,
        projectName: "devops-test-project/${params.branch}",
        selector: upstream()
      }
    }
    stage('Deploy With Ansible') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-pkey", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i ${DEPLOY_TO}.ini playbook.yml'
        }
      }
    }
    stage('Validate deployment with Newman') {
      steps {
        sh 'docker run -t postman/newman run https://www.getpostman.com/collections/b276ed088bb40033c483 -e myhost=${host_ip}
      }
    }
  }
}