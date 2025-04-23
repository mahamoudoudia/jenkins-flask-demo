pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mahamoudoudia/flask-ci-demo'
        DOCKER_HOST = 'tcp://<docker_host>:2375'  // Remplace <docker_host> par l'adresse de ton hôte Docker
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

        stage('Déployer sur Docker') {
            steps {
                script {
                    // Déploiement sur Docker (exemple avec Docker Compose ou docker run)
                    sh '''
                        docker pull $IMAGE_NAME
                        docker stop flask-ci-demo || true
                        docker rm flask-ci-demo || true
                        docker run -d --name flask-ci-demo -p 5000:5000 $IMAGE_NAME
                    '''
                }
            }
        }
    }
}
