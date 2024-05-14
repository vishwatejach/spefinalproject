pipeline {
    agent any
    tools {
        maven 'maven' 
    }
    environment{
        registry1='vishwatejach/mydiningbackend1'
        registry2='vishwatejach/mydiningfrontend'
        registry3='vishwatejach/mydiningdatabase'
        dockerImage1=''
        dockerImage2=''
        dockerImage3=''
    }
    stages {
        stage('Git Pull') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/A1ziz26/spefinalproject']])
            }
        }
        stage('Build Maven') {
            steps {
                dir('spebackend') {
                    sh 'mvn clean install'
                }
            }
        }
        stage('Maven Test') {
            steps {
                dir('spebackend') {
                    sh 'mvn test'
                }
            }
        }
        stage("Build and Push Docker Images"){
            steps{
                ansiblePlaybook(
                    installation: 'Ansible',
                    inventory: '/var/lib/jenkins/workspace/mydining/deploy/inventory',
                    playbook: '/var/lib/jenkins/workspace/mydining/deploy/deploy_docker.yaml',
                    vaultCredentialsId: 'ansible_vault'
                )
            }
        }
        stage("Run Docker Compose") {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
