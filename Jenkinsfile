pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf pipelinetesting_development'
            sh 'rm -rf pipelinetesting'
            sh 'git clone https://github.com/mavrick202/pipelinetesting.git'
            
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipelinetesting_development'
            sh 'docker build -t sreeharshav/pipelinetestdevelop:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push sreeharshav/pipelinetestdevelop:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.100:2375 stop devwebapp1 || true'
            sh    'docker -H tcp://10.1.1.100:2375 run --rm -dit --name devwebapp1 --hostname devwebapp1 -p 9000:80 sreeharshav/pipelinetestdevelop:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.100:9000'
          }
        }

    }
}

