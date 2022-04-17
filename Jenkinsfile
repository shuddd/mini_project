pipeline {
    environment{
        imageName=''
    }
    agent any

    stages {
        stage('Git pull!') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/shuddd/mini_project'
            }
        }
        stage('Maven Build') {
            steps {
                script{
                    sh 'mvn clean install'
                }
            }
        }
        stage('Docker Build to image') {
            steps {
                script{
                    imageName=docker.build "shuddd/mini_project:latest"
                }
            }
        }
        stage('Push Docker image') {
            steps {
                script{
                    docker.withRegistry('','docker-cred'){
                        imageName.push()
                    }
                }
            }
        }
        stage('Ansible pull docker image') {
            steps {
                    ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'deploy-docker/inventory', playbook: 'deploy-docker/calc-deploy.yml', sudoUser: null
            }
        }
    }
}
