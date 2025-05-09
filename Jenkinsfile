pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'rajadocker9/jenkins-application'
        DOCKER_CREDS_ID = 'dockerhub-creds'
    }

    stages {
        stage('Setup') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo 'Building NestJS app...'
                sh 'npm run build'
                sh 'ls dist' // verify build output
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm run test'
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDS_ID}") {
                        def app = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        app.push()
                    }
                }
            }
        }
    }
}
