pipeline {
    agent any

    stages {
        stage('Cloner') {
            steps {
                git 'https://github.com/mahamoudoudia/jenkins-flask-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('flask-ci-demo')
                }
            }
        }

        stage('Tests') {
            steps {
                sh 'echo "Tests fictifs pour le moment"'
            }
        }

        stage('Push Docker Image') {
            environment {
                DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image('flask-ci-demo').push('latest')
                    }
                }
            }
        }
    }
}
