pipeline {
    agent any 
    environment {
        TERRAFORM_DIR = 'D:\\Nouveau dossier\\terraform_1.6.4_windows_386'
        KUBECONFIG_PATH = 'C:\\Users\\msi\\.kube\\config'
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
                bat 'docker build -t mon-app .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                // Tag the Docker image
                bat 'docker tag mon-app:latest emnaboukhris/my_app_image:latest'
            }
        }

        stage('Push image to docker hub'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                        bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                        bat "docker push emnaboukhris/my_app_image:latest"
                    }
                }
            }
        }

        stage("Verification de kubectl") {
            steps {
                withCredentials([file(credentialsId: 'kube_credentials', variable: 'KUBECONFIG')]) {
                    echo "======== executing ========"
                    bat "kubectl"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    env.KUBECONFIG = KUBECONFIG_PATH
                    echo "KUBECONFIG path: \$KUBECONFIG"
                    
                    try {
                        bat "kubectl version"
                        bat "kubectl apply -f fichier-deployment.yaml"  
                    } catch (Exception e) {
                        error "Error executing kubectl command: ${e.getMessage()}"
                    }
                }
            }
        }

        stage('Creation des services') {
            steps {
                script {
                    env.KUBECONFIG = KUBECONFIG_PATH
                    echo "KUBECONFIG path: \$KUBECONFIG"
                    
                    try {
                        echo "* executing *"
                        bat "kubectl apply -f external-service-definition.yaml"
                        bat "kubectl get services"
                    } catch (Exception e) {
                        error "Error executing kubectl command: ${e.getMessage()}"
                    }
                }
            }
        }
        stage("Terraform Init") {
            steps {
                script {
                    dir(TERRAFORM_DIR) {
                        bat 'terraform init'
                    }
                }
            }
        }
        stage('Terraform Apply') {
            steps {
                script {
                    dir(TERRAFORM_DIR) {
                        bat 'terraform apply -auto-approve'
                    }
                }
            }
        }
    }
}