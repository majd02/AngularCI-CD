pipeline {
    agent any
    stages {
        stage ("Pull") {
            steps{
                script{
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        credentialsId: '0fbf5193-beb6-4355-9f4c-9ae153908350',
                        url: 'https://github.com/louhaichi/angular-achat-CICDproject.git']]])
                }
            }
        }

        stage('python test'){
            steps{
                script{
                    sh "python3 --version"
                }
            }
        }
        stage('Build'){
            steps{
                script{
                    sh "ANSIBLE_DEBUG=1 ansible-playbook ansible/build.yml -i ansible/inventory/host.yml -e ansible_become_password=ubuntu"
                }
            }
        }
        stage('docker'){
            steps{
                script{
                    sh "ansible-playbook ansible/docker.yml -i ansible/inventory/host.yml -e ansible_become_password=ubuntu"
                }
            }
        }
        stage('docker-registry'){
            steps{
                script{
                    sh "ansible-playbook ansible/docker-registry.yml -i ansible/inventory/host.yml -e ansible_become_password=ubuntu"
                }
            }
        }
    }
}