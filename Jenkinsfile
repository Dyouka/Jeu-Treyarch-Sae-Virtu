pipeline {
    agent any

    environment {
        REGISTRY = "10.1.102.51:5000"
        IMAGE_NAME = "myapp"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Dyouka/https://github.com/Dyouka/Jeu-Treyarch-Sae-Virtu.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REGISTRY/$IMAGE_NAME .'
            }
        }

        stage('Push to Docker Registry') {
            steps {
                sh 'docker push $REGISTRY/$IMAGE_NAME'
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sshagent(['deploy-key']) {
                    sh 'ansible-playbook /etc/ansible/deploy_app.yml'
                }
            }
        }
    }
}
