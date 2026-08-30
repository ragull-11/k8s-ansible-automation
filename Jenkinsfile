pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-ssh-key',
                    url: 'git@github.com:ragull-11/k8s-ansible-automation.git'
            }
        }

        stage('Ansible Ping Test') {
            steps {
                sshagent (credentials: ['ansible-ssh-key']) {
                    sh 'ansible all -i inventory.ini -m ping'
                }
            }
        }

        stage('Deploy NGINX Stack') {
            steps {
                sshagent (credentials: ['ansible-ssh-key']) {
                    sh 'ansible-playbook -i inventory.ini deploy-nginx-stack.yaml'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully — cluster configured via Ansible.'
        }
        failure {
            echo 'Pipeline failed — check the stage logs above.'
        }
    }
}
