pipeline {
    agent any

    environment {
        registry = "sanjanag20/jenkins-docker-sample"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    stages {
        stage ('Git Checkout') {
             steps {
                git branch: 'main', url: 'https://SHA256:bqo1Lk35ijcAa7wnKa+E203IC3ZgcT8EE8uFoeq1dxQ@github.com/sanjana-g20/jenkins-docker-sample.git'
            }
        }
        stage('Cloning Git') {
            steps {
                git "https://github.com/sanjana-g20/jenkins-docker-sample"
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // Example using Trivy for security scan
                    sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image ${registry}'
                }
            }
        }
        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                sh "docker rmi ${registry}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

