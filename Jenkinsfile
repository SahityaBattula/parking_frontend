pipeline {
    environment {
        registry = "sahityabattula/parking_ui2"
        registrycredential = 'dockerhub'
        dockerImage = ''
    }
       agent none
        stages {
            stage('SCM Checkout') {
                agent any
               steps{
                    git 'https://github.com/SahityaBattula/parking_frontend.git'
                }
            }
            stage('Build') {
                agent {
                    docker { image 'node:12-alpine' }
                }
                steps {
                    sh 'npm install'
                    sh 'npm run build'
                
                
                }
            }
            stage('Build Image') {
                agent any
                steps{
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }
            stage('Deploy') {
                agent any
                   steps{
                    script {
                        docker.withRegistry('',registrycredential ) {
                            dockerImage.push()
                        }
                    }
                   }
            }
            stage('removed unused docker image')
            agent any
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
                
                        
                    
            }
            }
        }

