pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mahamoudoudia/flask-ci-demo'
        DOCKER_HOST = 'tcp://92.222.217.60:2375'
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git url: 'https://github.com/mahamoudoudia/jenkins-flask-demo.git', branch: 'main'
            }
        }

        stage('Construire l\'image Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Tests') {
            steps {
                echo 'Tests en cours...'
            }
        }

        stage('Pousser l\'image sur Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Déployer sur Docker') {
            steps {
                script {
                    sh '''
                        docker -H $DOCKER_HOST pull $IMAGE_NAME
                        docker -H $DOCKER_HOST stop flask-ci-demo || true
                        docker -H $DOCKER_HOST rm flask-ci-demo || true
                        docker -H $DOCKER_HOST run -d --name flask-ci-demo -p 5000:5000 $IMAGE_NAME
                    '''
                }
            }
        }
    }
}
