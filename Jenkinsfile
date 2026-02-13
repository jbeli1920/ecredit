pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hamdi1920/backend:latest'
        DOCKER_NETWORK = 'ecredit-network'
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
        SONAR_URL = 'http://localhost:9000'
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'safe-branch', url: 'https://github.com/jbeli1920/ecredit'
            }
        }

        stage('Build Maven') {
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
                    # Create network if not exists
                    docker network create ${DOCKER_NETWORK} || true
                    # Stop and remove old containers (if any)
                    docker compose down || true
                    # Start containers
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
