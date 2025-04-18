@Library('docker') _

pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        IMAGE_NAME = 'mahamoudoudia/flask-ci-demo'
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'main', url: 'https://github.com/mahamoudoudia/jenkins-flask-demo.git'
            }
        }

        stage('Construire l\'image Docker') {
            steps {
                script {
                    def app = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Tests') {
            steps {
                sh 'echo "Exécution des tests (à implémenter)"'
            }
        }

        stage('Pousser l\'image sur Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
    }
}
