pipeline {
    agent any
 
    environment {
        APP_NAME = 'new-app-nti' 
        REPO_URL = "https://github.com/mennatallah777/multi-branch.git"
    }

    stages {
        stage('Getting Repo files') {
            steps {
                git branch: "${GIT_BRANCH}", credentialsId: '516b5e1c-0443-47b1-bf1a-1e41058257d3', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build -t ${APP_NAME}:${BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {

                        sh """
                            echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin

                            docker tag ${APP_NAME}:${BUILD_NUMBER} ${DOCKER_USERNAME}/${APP_NAME}:${BUILD_NUMBER}

                            docker push ${DOCKER_USERNAME}/${APP_NAME}:${BUILD_NUMBER}
                        """

                    }
                }
            }
        }
    }
}
