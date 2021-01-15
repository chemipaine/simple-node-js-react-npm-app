pipeline {
    agent none
    environment {
        CI = 'true' 
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:6-alpine'
                    args '-p 3000:3000'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            agent {
                docker {
                    image 'node:6-alpine'
                    args '-p 3000:3000'
                }
            }
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('CreateImage') { 
            agent {
                label 'master'
            }
            steps {
                sh 'docker build -t simplenodejs:latest .' 
                sh 'docker tag simplenodejs:latest chemipaine/simplenodejs:latest'
                sh 'docker push chemipaine/simplenodejs:latest'
            }
        }
        stage('Deploy') { 
            agent {
                label 'master'
            }
            steps {
                sh 'kubectl apply -f kubernetes' 
            }
        }
        
    }
}