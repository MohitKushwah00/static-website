pipeline {
    agent any

    environment {
        DOCKER_IMAGE = '7696141945/stat-website:latest'
        REPO_URL = 'https://github.com/MohitKushwah00/static-website.git'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo '🧹 Cleaning workspace...'
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                echo '🔄 Cloning GitHub repo...'
                sh 'git clone $REPO_URL'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE ./static-website'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo '📤 Pushing Docker image to DockerHub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Deploying to Kubernetes cluster...'
                sh '''
                    kubectl apply -f deployment.yaml
                    
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Build or deployment failed. Check logs above.'
        }
    }
}
