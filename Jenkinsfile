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
                    // Clean up any existing containers and images
                    sh '''
                        docker-compose -p todo-pipeline down --remove-orphans
                        docker system prune -f
                    '''
                    
                    // Build the new image
                    sh 'docker-compose -p todo-pipeline build --no-cache'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Deploy and ensure it stays running
                    sh '''
                        docker-compose -p todo-pipeline down
                        docker-compose -p todo-pipeline up -d
                        # Wait for container to start
                        sleep 10
                        # Verify container is running
                        docker-compose -p todo-pipeline ps
                        # Check logs
                        docker-compose -p todo-pipeline logs
                    '''
                }
            }
        }
    }
    
    post {
        failure {
            sh 'docker-compose -p todo-pipeline logs'
        }
    }
}