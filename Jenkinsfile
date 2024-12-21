pipeline {
    agent { label 'node-agent' }
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/DanPhillipsKW/node-todo-cicd.git', branch: 'master' 
            }
        }
        stage('Build and Test'){
            steps{
                sh 'docker buildx build . -t dannyp/node-todo-test:latest'
            }
        }
        /* Commenting out Docker Hub push stage for now
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                 sh 'docker push dannp/node-todo-test:latest'
                }
            }
        }
        */
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}