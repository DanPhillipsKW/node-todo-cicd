pipeline {
    agent { label 'ToDo' }
    
    stages {
        stage('Code') {
            steps {
                echo "Starting Code stage"
                cleanWs()
                git url: 'https://github.com/DanPhillipsKW/node-todo-cicd.git', branch: 'master'
            }
        }
        
        stage('Build and Test') {
            steps {
                script {
                    echo "Starting Build stage..."
                    sh 'docker rm -f node-todo-app || true'
                    sh 'docker buildx build . -t todo-node-app --no-cache'
                    sh 'docker run -d --name node-todo-app -p 8000:8000 todo-node-app'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh "docker-compose down || true"
                    sh "docker-compose up -d"
                }
            }
        }
    }
}