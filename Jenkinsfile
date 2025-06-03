pipeline{
    agent any
    environment{
        registry = 'noeury/eoi-act3-jenkins'
        registryCredentials = 'dockerhub'
        project='eoi-ci-cd'
        projectVersion = '1.0.0'
        repository = 'https://github.com/Noeury/eoi-ci-cd.git'
        repositoryCredentials = 'github'
    }
    stages{
        stage('Clean Workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout code'){
            steps{
                script {
                    git branch: 'main',
                        credentialsId: repositoryCredentials,
                        url: repository
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script {
                    dockerImage= docker.build registry
                }
            }
        }
        stage('Test'){
            steps{
                script{
                    try{
                        bat 'docker run --name %project% %registry%'
                    }
                    finally{
                        bat 'docker rm -f %project%'
                    }
            }
            }
        }
        stage('Push Docker Image'){
            steps{
                script {
                    docker.withRegistry("", registryCredentials) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up'){
            steps{
                script{
                    bat 'docker rmi %registry%'
                }
            }
        }
    }
    post{
        always{
            echo 'Registrar build'
        }
    }
}