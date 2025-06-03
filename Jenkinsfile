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
                    dockerImage= docker.build("${registry}:${projectVersion}")
                }
            }
        }
        stage('Test'){
            steps{
                script{
                    try{
                        sh 'docker run --name $project $registry'
                    }
                    finally{
                        sh 'docker rm -f $project'
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
                    sh 'docker rmi $registry'
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