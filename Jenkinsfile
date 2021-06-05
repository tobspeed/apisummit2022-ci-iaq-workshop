pipeline {
    agent any

    environment {
      HCLOUD_TOKEN = credentials('hcloud-token')
    }

    stages {
        stage('Start Test VM') {
            steps {
              dir('ci-test-vm') {
                sh 'terraform init'
                // hier soll die VM gestartet werden
              }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
              sh 'ansible-galaxy collection install -r requirements.yml'
              // hier sollen die Playbooks laufen
            }
        }
    }
    post {
        always {
          dir('ci-test-vm') {
             // hier soll die VM gelöscht werden
          }
         }
    }
}
