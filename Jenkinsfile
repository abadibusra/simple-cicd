pipeline {
    agent any
    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Setup') {
	    steps {
		sh '''
		    python3 -m venv .venv
		    . .venv/bin/activate
		    pip install -r requirements.txt -r requirements-dev.txt
		'''
	    }
	}
	stage('Lint') {
	    steps {
		sh '''
		    . .venv/bin/activate
		    ruff check app/ tests/
		'''
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
        stage('Deploy') {
            steps {
                sh '''
                    docker stop myapp || true
                    docker rm myapp || true
                    docker run -d -p 5000:5000 --name myapp simple-cicd:${BUILD_NUMBER}
                '''
            }
        }

    }
}


