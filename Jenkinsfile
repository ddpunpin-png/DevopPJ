pipeline {

    agent any

    environment {
        IMAGE = "ghcr.io/ddpunpin-png/devoppj"
        TAG = "${env.BUILD_NUMBER}"
        REGISTRY_CREDS = "ghcr-creds"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/ddpunpin-png/DevopPJ.git',
                    branch: 'main'
            }
        }


        stage('Setup') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }


        stage('Train + Gate') {
            steps {
                sh '. venv/bin/activate && python3 train.py'
            }
        }


        stage('Test') {
            steps {
                sh '. venv/bin/activate && pytest -q'
            }
        }


        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE}:${TAG} -t ${IMAGE}:latest ."
            }
        }


        stage('Push Image') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: "${REGISTRY_CREDS}",
                        usernameVariable: 'REG_USER',
                        passwordVariable: 'REG_PASS'
                    )
                ]) {

                    sh 'echo "$REG_PASS" | docker login ghcr.io -u "$REG_USER" --password-stdin'

                    sh "docker push ${IMAGE}:${TAG}"

                    sh "docker push ${IMAGE}:latest"
                }
            }
        }
    }


    post {
        success {
            echo "Pipeline completed successfully"
        }

        failure {
            echo "Pipeline failed"
        }
    }
}
