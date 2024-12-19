pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    def port = branch == 'main' ? '3000' : '3001'
                    def imageName = "my-app:${branch}"
                    sh "docker build -t ${imageName} ."
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    def port = branch == 'main' ? '3000' : '3001'
                    def imageName = "my-app:${branch}"
                    sh "docker run -d -p ${port}:${port} ${imageName}"
                }
            }
        }
    }
}
