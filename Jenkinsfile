pipeline {
    agent any

    stages {
        stage('Checkout GitHub repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/imransetiadi/hellodocker-cicd.git']])
            }
        }
        
        stage('Build and Tag Docker Image') {
            steps {
                script {
                    sh 'docker build . -t hellodocker'
                    sh 'docker tag hellodocker imransetiadi22/hellodocker'
                }
            }
        }
        
        stage('Push the Docker Image to DockerHUb') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub')]) {
                    sh 'docker login -u jamesndubuisi -p ${docker_hub}'
}
                    sh 'docker push imransetiadi22/hellodocker'
                }
            }
        }
        
        stage('Deploy deployment and service file') {
            steps {
                script {
                    kubernetesDeploy configs: 'deploymentsvc.yaml', kubeconfigId: 'kubernetes_config'
                }
            }
        }
    }
}
