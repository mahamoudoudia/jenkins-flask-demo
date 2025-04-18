pipeline {
    agent any
 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
    }
 
    stages {
        stage('Initialize') {
            steps {
                script {
                    def dockerHome = tool 'myDocker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
            }
        }
 
        stage('Cloner le dépôt') {
            steps {
                git url: 'https://github.com/mahamoudoudia/jenkins-flask-demo.git', branch: 'main'
            }
        }
 
        stage('Construire l\'image Docker') {
            steps {
                sh 'docker build -t mahamoudoudia/flask-ci-demo .'
            }
        }
 
        stage('Tests') {
            steps {
                echo 'Tests en cours...'
                // Ajoute ici les tests si nécessaire
            }
        }
 
        stage('Pousser l\'image sur Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push mahamoudoudia/flask-ci-demo
                    """
                }
            }
        }
    }
}
 
