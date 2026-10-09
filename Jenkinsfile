pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "apekshagowda56/401"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/apekshagowda56/app10.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${img1}:v1")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        docker.image("${img1}:v1").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
