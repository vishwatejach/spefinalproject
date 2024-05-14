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
        stage('Build Docker Images') {
            steps {
                script {
                    dir('spebackend') {
                        dockerImage1 = docker.build registry1
                    }
                    dir('spefrontend') {
                        dockerImage2 = docker.build registry2 
                    }
                    dockerImage3 = docker.build registry3 
                }
            }
        }
        stage("Push to DockerHub"){
            steps{
                script{
                    docker.withRegistry('','dockerhub-pwd'){
                    dockerImage1.push()
                    dockerImage2.push()
                    dockerImage3.push()
                    }
                }
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