pipeline {
    agent any

    stages {
        stage('Run Ansible - Test Channels') {
            steps {
                sshagent(credentials: ['ansible-ssh']) {
                    sh '''
                        ansible-playbook -i inventory.ini playbooks/test_channels.yml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Playbook test_channels exécuté avec succès !'
        }
        failure {
            echo '❌ Erreur lors de l’exécution du playbook !'
        }
    }
}
