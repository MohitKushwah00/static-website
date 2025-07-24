pipeline {
    agent any

    environment {
        IMAGE_NAME = "static-website"
        DOCKER_USER = "7696141945"   // Replace with your Docker Hub username if pushing
        DEPLOYMENT_FILE = "deployment.yaml"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/MohitKushwah00/static-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh "docker tag ${IMAGE_NAME} ${DOCKER_USER}/${IMAGE_NAME}:latest"
            }
        }

        // Optional: Push to DockerHub if needed
        // stage('Push Image') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        //             sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
        //             sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:latest"
        //         }
        //     }
        // }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f ${DEPLOYMENT_FILE}"
            }
        }
    }
}
