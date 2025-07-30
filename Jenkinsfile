pipeline {
    agent any

    environment {
        IMAGE_NAME = "7696141945/static-website"
        TAG = "latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:$TAG ."
                }
            }
        }
    }

    post {
        failure {
            echo 'Build failed. Please check the logs.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}
