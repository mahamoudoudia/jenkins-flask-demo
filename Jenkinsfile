pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mahamoudoudia/flask-ci-demo'
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
                // Ajoute ici les commandes de test si besoin
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
    }
}
