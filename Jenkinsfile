pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vijay254452/realestate.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t dreamhomes:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:8080 dreamhomes:latest'
            }
        }
    }
}

