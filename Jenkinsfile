pipeline {
    agent any

    environment {
        VAULT_ADDR = 'http://127.0.0.1:8200'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "✅ Code récupéré depuis Git"
            }
        }

        stage('Channels') {
            steps {
                withCredentials([string(credentialsId: 'vault-token', variable: 'VAULT_TOKEN')]) {
                    sshagent(credentials: ['ansible-ssh']) {
                        sh '''
                            ansible-playbook -i inventory.ini playbooks/create_channels.yml
                        '''
                    }
                }
            }
        }

        stage('Groups') {
            steps {
                withCredentials([string(credentialsId: 'vault-token', variable: 'VAULT_TOKEN')]) {
                    sh '''
                        ansible-playbook -i inventory.ini playbooks/create_groups.yml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Channels et groupes créés avec succès !'
        }
        failure {
            echo '❌ Erreur pendant le pipeline !'
        }
    }
}
