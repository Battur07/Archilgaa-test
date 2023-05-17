pipeline {
    agent any
    tools{
        git 'GIT' 
    }
    stages {
        stage('Кодыг татах хэсэг') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Battur07/Archilgaa-test.git']])
            }
        }
        stage('Docker Image үүсгэж буй хэсэг') {
            steps {
                script {
                    sh 'sudo docker build -t docker-image1:latest .'
                }
            }
        }
        stage('Docker Image Deploy хийх хэсэг') {
            steps {
                script {
                    def containerExists = sh(script: 'sudo docker ps -a | grep docker-image1-service', returnStatus: true) == 0
                    if (containerExists) {
                        sh 'sudo docker stop docker-image1-service'
                        sh 'sudo docker rm docker-image1-service'
                    }
                   
                }
             def CONTAINER_NAME = 'docker-image1-service'
                  
                    if (sh(returnStatus: true, script: "sudo docker ps -a --format '{{.Names}}' | grep -Eq '^${CONTAINER_NAME}$'")) {
                      
                        sh "sudo docker stop ${CONTAINER_NAME}"
                        sh "sudo docker rm ${CONTAINER_NAME}"
                    }
                    sh "sudo docker run -p 3000:3000 --name ${CONTAINER_NAME} --detach --rm docker-image1:latest"
            }
        }
    }
}
