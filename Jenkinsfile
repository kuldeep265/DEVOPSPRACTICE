pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'kuldeep7860/mavenimg'
    }

    stages {

        stage('clone repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kuldeep265/DEVOPSPRACTICE.git'
            }
        }

        stage('maven build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('build docker image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('docker login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds2',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat 'echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin'
                }
            }
        }

        stage('docker push') {
            steps {
                bat "docker push %DOCKER_IMAGE%"
            }
        }

        stage('deploy') {
            steps {
                bat 'docker compose down'
                bat 'docker compose up -d'
            }
        }
    }

    post {
        success {
            echo 'Pipeline Successful'
        }
        failure {
            echo 'Pipeline Failed'
        }
    }
}
