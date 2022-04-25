pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'prod', 'cloud'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
    string(name: 'branch', defaultValue: '', description: 'branch to copy artifact from')
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
    stage('Run Healthcheck') {
      steps {             
          sh "curl -sf ${host_ip}:8080 >/dev/null"
      }
    }
  }
}