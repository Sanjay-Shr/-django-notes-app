@Library("Shared") _
pipeline {
    agent { label "mango" }

    stages {
        stage("Hello") {
            steps {
                script {
                    hello() 
                }
            }
        }
        stage("Code") {
            steps {
                script{
                    clone("https://github.com/Sanjay-Shr/-django-notes-app","main")   
                }
            }
        }   
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t notes-app:latest ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
               script{
                   docker_push("notes-app","latest","sanjayshr944811")
               }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the code"
                sh '''
                    echo "Stopping existing containers..."
                    docker-compose down || true
                    
                    echo "Starting new containers..."
                    docker-compose up -d --build
                    
                    echo "Deployment completed successfully."
                '''
            }
        }
    }
}
