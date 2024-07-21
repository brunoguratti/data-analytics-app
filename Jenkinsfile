pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Create and activate a virtual environment if it doesn't exist
                    def venvExists = fileExists('venv/bin/activate')
                    if (!venvExists) {
                        sh 'python3 -m venv venv'
                    }
                    sh '. venv/bin/activate'
                    sh 'pip install -r requirements.txt'
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build('bguratti/my-python-app:latest', '.')
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        docker.image('bguratti/my-python-app:latest').push('latest')
                    }
                }
            }
        }
        stage('Start Minikube') {
            steps {
                script {
                    sh 'minikube start'
                }
            }
        }
        stage('Deploy to Minikube') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }
    }
}
