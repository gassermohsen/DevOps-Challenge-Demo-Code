pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                git 'https://github.com/abdelkhalek97/Final-Task-Application.git'
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker build -f Dockerfile --network host -t abdelkhalek97/app:v1 .
                docker push abdelkhalek97/app:v1
                """
            
            }
            }
            
        }
        stage('CD'){
            steps{
                     withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                cd K8s-files/
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
