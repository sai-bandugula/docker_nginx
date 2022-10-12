pipeline {
    agent any  
      stages {
        stage('Clone Repo') {
          steps {
            sh 'rm -rf shanmukhashan022'
            sh 'https://github.com/sai-bandugula/docker_nginx.git'
            }
        }

        stage('Build Docker Image') {
            steps {
              sh 'docker build -t bsainath123/sairepo:${BUILD_NUMBER} .'
              }
        }
        stage('Push Docker image') {
            environment {
                DOCKER_HUB_LOGIN = credentials('docker')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh    'docker push bsainath123/sairepo:${BUILD_NUMBER}'
            }
        }
          
        stage('K8S Deploy') {
            steps {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                    sh ('kubectl apply -f  kube.yaml')
                }
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://43.204.36.247/:30007'
          }
        }
      }
}
