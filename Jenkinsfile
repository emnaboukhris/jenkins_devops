pipeline {
    agent any
    environment {
       DOCKER_HOME = 'C:\\Program Files\\Docker\\Docker\\resources\\bin'
    }
    tools {
        // Specify the name of the Docker tool configured in Jenkins
        dockerTool 'docker'
    }
    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code depuis GitHub
                checkout([$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/emnaboukhris/jenkins_devops']]])
            }
        }
        stage('Build Docker Image') {
            steps {
                // Construire l'image Docker
                sh 'docker build -t mon-app .'
            }
        }
        stage('Tag Docker Image') {
            steps {
                // Tag the Docker image
                sh 'docker tag mon-app:latest emnaboukhris/my_app_image:latest'
            }
        }
        stage('Push to Docker Hub') {
            steps {

                // Se connecter à Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD'
                }
                // Pousser l'image vers Docker Hub
                sh 'docker push emnaboukhris/my_app_image:latest'
            }
        }
        stage('Deploy on k8s') {
             steps {
                 script {
                  // Assuming your Kubernetes manifests are in the same directory as your pipeline script
                  sh 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl apply -f fichier-deployment.yaml -f external-service-definition.yaml'
                }
            }
        }

    }
}
