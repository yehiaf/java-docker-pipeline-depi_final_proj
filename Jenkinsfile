pipeline {
    agent { node 'agent1' }

    environment {
        DOCKER_HUB_USERNAME = 'yehiaf'
        DOCKER_HUB_PASSWORD = credentials('dockerhub-credentials').getPassword()
        IMAGE_NAME = 'java-app-pipeline'
        IMAGE_TAG = 'latest' 
    }

    stages {
        stage('Build Image') {
            agent { node 'agent1' }
            steps {
                script {
                    sh 'npm install' // if we want to Build the app inside the image
                    sh "docker build -t $DOCKER_HUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", "dockerhub-credentials") {
                        sh "docker push $DOCKER_HUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG"
                    }
                }
            }
        }

        stage('Deploy and Test') {
            agent { label 'your-deployment-agent' } //put here our specific agent for deployment
            steps {
                script {
                    sh "docker pull $DOCKER_HUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG"
                    sh "docker run -d -p 80:8080 $DOCKER_HUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG" 
                    sleep 10 // Wait for the container to start
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://localhost", returnStdout: true).trim()
                    if (response != '200') {
                        error("Website is not accessible. Received HTTP response: ${response}")
                    } else {
                        echo "Website is accessible. Received HTTP response: ${response}"
                    }
                }
            }
        }
    }

    post {
        always {
            agent { none() } // No agent needed for cleanup
            sh 'docker rm -f $(docker ps -aq) || true'
            sh 'docker rmi $DOCKER_HUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG || true'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
