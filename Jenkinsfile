pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'prod'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
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
    stage('Check Availability') {
      steps {             
          waitUntil {
              try {         
                  sh "curl -s --head --request GET ${host_ip}:8080 | grep '200'"
                  return true
              } catch (Exception e) {
                    return false
              }
          }
        }
    }
  }
}