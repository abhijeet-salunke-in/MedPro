pipeline {

    agent any

    environment {
        DOCKER_USERNAME = 'YOUR_DOCKERHUB_USERNAME'
        APP_IMAGE = "${DOCKER_USERNAME}/medpro"

        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/abhijeet-salunke-in/MedPro.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                    -t ${APP_IMAGE}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker_hub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USER" \
                        --password-stdin
                    '''

                    sh '''
                        docker push ${APP_IMAGE}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'MedPro pipeline completed successfully.'
        }

        failure {
            echo 'MedPro pipeline failed.'
        }
    }
}
