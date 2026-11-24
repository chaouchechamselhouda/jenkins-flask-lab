
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/chaouchechamselhouda/jenkins-flask-lab.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -f "$WORKSPACE/Dockerfile" -t jenkins-flask-app "$WORKSPACE"'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm jenkins-flask-app python -m pytest -v'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f jenkins-flask-container || true
                    docker run -d --name jenkins-flask-container -p 8081:8000 jenkins-flask-app
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'sleep 5; curl -f http://localhost:8081/health'
            }
        }
    }
}
