pipeline {
    agent any
    environment{
        DOCKERHUB_CREDENTIALS = credentials('docker')
        SHOW_CONT = '$(docker ps -aq)'
    }
    stages {
        stage('Clean-up'){
            steps {
                sh "docker rm -f ${SHOW_CONT} || true"
                sh "docker stop mynginx || true"
                sh "docker rm mynginx || true"
            }
        }
        stage('Build'){
            steps {
                sh "chmod +x deploy.sh"
                sh "./deploy.sh"
            }
        }
        stage('Deploy'){
            steps {
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "docker tag trio-task-mysql:5.7 cullenge/mytriotasksql1:latest"
                sh "docker tag trio-task-flask-app cullenge/mytriotaskflask1:latest"
                sh "docker push cullenge/mytriotasksql1:latest"
                sh "docker push cullenge/mytriotasksql1:latest"
            }
        }
    }
}