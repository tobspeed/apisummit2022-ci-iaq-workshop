pipeline {
    agent any
    stages {
        stage('Ansible Collection Preparation') {
            steps {
              sh 'ansible-galaxy collection install -r requirements.yml'
            }
        }
        stage('Start Test VM') {
            steps {
                sh 'vagrant up'
            }
        }
        stage('Run Ansible Playbook') {
            steps {
              sh 'ansible-playbook -i inventories/vagrant install-hero-app.yml'
            }

        }
    }
    post {
        always {
            sh 'vagrant destroy -f'
        }
    }
}
