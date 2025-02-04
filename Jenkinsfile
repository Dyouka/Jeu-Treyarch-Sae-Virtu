pipeline {
    agent any

    environment {
        REGISTRY = "10.1.102.51:5000"  // IP de ta VM Docker Registry
        IMAGE_NAME = "myapp"
        DEPLOY_SERVER = "10.1.102.50"  // IP de ta VM de déploiement
        SSH_USER = "rt"  // Ton utilisateur SSH
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Dyouka/Jeu-Treyarch-Sae-Virtu.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REGISTRY/$IMAGE_NAME .'
            }
        }

        stage('Push to Registry') {
            steps {
                sh 'docker push $REGISTRY/$IMAGE_NAME'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sshagent(['deploy-key']) {
                    sh '''
                    ssh $SSH_USER@$DEPLOY_SERVER "ansible-playbook /home/$SSH_USER/deploy_app.yml"
                    '''
                }
            }
        }
    }
}
