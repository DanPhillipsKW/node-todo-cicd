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
                    // Stop and remove any existing containers
                    sh '''
                        docker ps -q --filter "name=node-todo-app" | grep -q . && docker stop node-todo-app || true
                        docker ps -aq --filter "name=node-todo-app" | grep -q . && docker rm -f node-todo-app || true
                        docker ps -q --filter "name=todo-pipeline" | grep -q . && docker stop todo-pipeline || true
                        docker ps -aq --filter "name=todo-pipeline" | grep -q . && docker rm -f todo-pipeline || true
                    '''
                    sh 'docker buildx build . -t todo-node-app --no-cache'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Stop all running containers from previous deployments
                    sh '''
                        docker-compose down --remove-orphans
                        sleep 5
                        docker-compose up -d
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo "Cleaning up..."
            sh '''
                docker-compose down --remove-orphans || true
                docker ps -q --filter "name=node-todo-app" | grep -q . && docker stop node-todo-app || true
                docker ps -aq --filter "name=node-todo-app" | grep -q . && docker rm -f node-todo-app || true
            '''
        }
    }
}