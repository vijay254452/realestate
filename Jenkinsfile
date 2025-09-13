pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Jenkins credentials ID
        DOCKER_IMAGE = "vijay3247/realestate"
        BRANCH = "main"
        WAR_NAME = "real-estate-website-1.0-SNAPSHOT.war"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${BRANCH}",
                    url: 'https://github.com/vijay254452/realestate.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Ensure Dockerfile is present in repo root
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"

                    // Also tag as latest
                    sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker rm -f realestate || true"
                    sh "docker run -d --name realestate -p 6777:8080 ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully! Visit http://<your-server-ip>:8080/"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}

