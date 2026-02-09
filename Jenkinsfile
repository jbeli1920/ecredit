pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hamdi1920/ecredit-backend:latest'
        DOCKER_NETWORK = 'ecredit-network'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jbeli1920/ecredit'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Docker Up') {
            steps {
                sh """
                    docker network create ${DOCKER_NETWORK} || true
                    docker compose up -d
                """
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
