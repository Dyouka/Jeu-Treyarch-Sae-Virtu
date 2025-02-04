pipeline {
    agent any

    environment {
        REGISTRY = "10.1.102.51:5000"
        IMAGE_NAME = "myapp"
        DEPLOY_SERVER = "10.1.102.48"
        SSH_USER = "rt"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Dyouka/Jeu-Treyarch-Sae-Virtu.git'
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
                    ssh "-o StrictHostKeyChecking=no" $SSH_USER@$DEPLOY_SERVER "ansible-playbook /home/$SSH_USER/deploy_app.yml"
                    '''
                }
            }
        }
    }
}
