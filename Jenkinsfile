pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                git 'https://github.com/gassermohsen/DevOps-Challenge-Demo-Code'
                withCredentials([usernamePassword(credentialsId: 'Docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker build -f Dockerfile --network host -t gassermohsen/app:v1 .
                docker push gassermohsen/app:v1
                """
            
            }
            }
            
        }
        stage('CD'){
            steps{
                     withCredentials([usernamePassword(credentialsId: 'Docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                kubectl create namespace app
                kubectl apply -f deployment-redis.yml
                kubectl apply -f redis-service.yml
                kubectl apply -f app-deployment.yml
                kubectl apply -f lb.yml
                kubectl get svc
                """
                     }
            }
        }
        
    }
}
