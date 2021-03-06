pipeline {
    
    agent any
    
    environment {
        dockerImage = ''
        registry = 'kovacsbenc3/test'
        registryCredential = 'kovacsbenc3'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kovacsbenc3/my-first-repository.git']]])
            }
        }
        
        stage('Build Docker image') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        
        stage ('Uploading Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                    dockerImage.push()
                    }
                }
            }
        }
        
        stage('docker stop container') {
            steps {
               sh 'docker ps -f name=testContainer -q | xargs --no-run-if-empty docker container stop'
               sh 'docker container ls -a -fname=testContainer -q | xargs -r docker container rm'
         }
       }
    
        stage('Docker Run') {
            steps{
                script {
                dockerImage.run("-p 8096:3000 --rm --name testContainer")
        }
      }
    }
  }
}