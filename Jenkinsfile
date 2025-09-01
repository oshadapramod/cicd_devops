pipeline {
    agent any
    
    stages {
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/oshadapramod/cicd_devops'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t oshadapramod/mydevopsproject:${BUILD_NUMBER} ."
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'devopspassword', variable: 'devopspassword')]) {
                    script {  
                        sh "docker login -u oshadapramod -p ${devopspassword}"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                sh "docker push oshadapramod/mydevopsproject:${BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}