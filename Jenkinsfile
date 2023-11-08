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
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/emnaboukhris/jenkins_devops']])

            }
        }
        stage('Build Docker Image') {
            steps {
                // Construire l'image Docker
                script {
                    // Use the DOCKER_HOME environment variable
                    def dockerCmd = "${DOCKER_HOME}\\docker"
                    sh "${dockerCmd} build -t mon-app ."
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Se connecter à Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    script {
                        // Use the DOCKER_HOME environment variable
                        def dockerCmd = "${DOCKER_HOME}\\docker"
                        sh "${dockerCmd} login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                    }
                }
                // Pousser l'image vers Docker Hub
                script {
                    // Use the DOCKER_HOME environment variable
                    def dockerCmd = "${DOCKER_HOME}\\docker"
                    sh "${dockerCmd} push mon-app"
                }
            }
        }
    }
}

            }
        }
        stage('Build Docker Image') {
            steps {
                // Construire l'image Docker
                script {
                    // Use the DOCKER_HOME environment variable
                    def dockerCmd = "${DOCKER_HOME}\\docker"
                    sh "${dockerCmd} build -t mon-app ."
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Se connecter à Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    script {
                        // Use the DOCKER_HOME environment variable
                        def dockerCmd = "${DOCKER_HOME}\\docker"
                        sh "${dockerCmd} login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                    }
                }
                // Pousser l'image vers Docker Hub
                script {
                    // Use the DOCKER_HOME environment variable
                    def dockerCmd = "${DOCKER_HOME}\\docker"
                    sh "${dockerCmd} push mon-app"
                }
            }
        }
    }
}
