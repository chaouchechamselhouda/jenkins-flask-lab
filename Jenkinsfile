
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
                sh '''
                    BUILD_DIR=/home/jenkins/jenkins-flask-build
                    rm -rf "$BUILD_DIR"
                    mkdir -p "$BUILD_DIR"
                    cp Dockerfile app.py requirements.txt "$BUILD_DIR"/
                    cp -r tests "$BUILD_DIR"/
                    docker build -t jenkins-flask-app "$BUILD_DIR"
                '''
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

