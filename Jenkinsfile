pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'mennayasser777/hello-app'
    }

    stages {
        stage('Env') {
            steps {
                sh 'echo "Build number: ${BUILD_NUMBER}"'
            }
        }

        stage('Checkout Repo') {
            steps {
                git branch: 'main', credentialsId: 'github-creds', url: "${REPOSITORY_URL}"
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Tag') {
            steps {
                sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
            }
        }

        stage('Push') {
            steps {
                sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }

        stage('Run') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
    }
}
