pipeline {
    agent {
        label 'docker-agent-alpine'
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
                // Build the Docker image
                script {
                    docker.build('my_app_image3', '.')
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Connect to Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    sh "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                }
                // Push the image to Docker Hub
                sh 'docker push my_app_image3'  // Use the correct image name
            }
        }
    }
}
