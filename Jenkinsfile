pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/abadibusra/simple-cicd.git'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install -r requirements.txt -r requirements-dev.txt
                    pytest -v
                '''
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t simple-cicd:${BUILD_NUMBER} .'
            }
        }

    }
}


