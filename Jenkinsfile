pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf pipelinetesting'
            sh 'git clone https://github.com/mavrick202/pipelinetesting.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipelinetesting'
            sh 'cp  /var/lib/jenkins/workspace/pipelinetesting/pipelinetesting/* /var/lib/jenkins/workspace/pipelinetesting'
            sh 'docker build -t sreeharshav/pipelinetesting:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push sreeharshav/pipelinetesting:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.100:2375 stop testwebapp1 || true'
            sh    'docker -H tcp://10.1.1.100:2375 run --rm -dit --name testwebapp1 --hostname testwebapp1 -p 10000:80 sreeharshav/pipelinetesting:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.100:10000'
          }
        }

    }
}

