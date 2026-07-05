pipeline {
    agent any 

    stages {
        stage('Checkout') { 
            steps {
                // This pulls your code from GitHub
                checkout scm
            }
        }
        stage('Build') { 
            steps {
                // This runs the build command you just perfected
                sh 'docker build -t devoppj:latest .'
            }
        }
        stage('Test') { 
            steps {
                // This runs your tests automatically
                sh 'python -m pytest test_app.py'
            }
        }
        stage('Deploy') { 
            steps {
                // This cleans up old containers and starts the new one
                sh 'docker rm -f devoppj || true'
                sh 'docker run -d -p 5000:5000 --name devoppj devoppj:latest'
            }
        }
    }
}
