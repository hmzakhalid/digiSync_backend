pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                git credentialsId: 'dhub_iftikhar', url: 'https://github.com/mlopProject/digiSync_backend'
            }
        }
        
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build("anashameed/digisync_api:latest")
                }
            }
        }
        
        stage('Push image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: "dockerHubCred", url: ""]) {
                        dockerImage.push()
                    }
                }
            }
        }
        
        stage('Run image') {
            steps {
                sh 'docker stop weather-app || true'
                sh 'docker rm weather-app || true'
                sh 'docker run -d --name weather-app -p 5000:5000 anashameed/digisync_api:latest'
            }
        }
    }
}