pipeline {
    agent { label 'ToDo' }
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/DanPhillipsKW/node-todo-cicd.git', branch: 'master' 
            }
        }
        stage('Build and Test'){
            steps{
                // Clean up any existing container
                sh 'docker rm -f node-todo-app || true'
                // Build using buildx
                sh 'docker buildx build . -t todo-node-app'
                // Run the new container
                sh 'docker run -d --name node-todo-app -p 8000:8000 todo-node-app'
            }
        }
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}