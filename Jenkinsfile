#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Copy Artifact') {                
            steps {      
                copyArtifact filter: 'devops'
            }            
        }
        stage('Deliver') {                
            steps {      
                sh 'scp ./devops vagrant@10.10.50.3'
            }            
        }
    }
}