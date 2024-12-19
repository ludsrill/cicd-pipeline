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
        stage('Push to Docker Hub') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    def imageName = branch == 'main' ? 'your-dockerhub-username/nodemain:v1.0' : 'your-dockerhub-username/nodedev:v1.0'

                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "${DOCKER_PASS}" | docker login -u "${DOCKER_USER}" --password-stdin
                            docker tag ${imageName} ${imageName}
                            docker push ${imageName}
                        """
                    }
                }
            }
        }
        stage('Trigger Deployment Pipeline') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    def deployJob = branch == 'main' ? 'Deploy_to_main' : 'Deploy_to_dev'

                    build job: deployJob, wait: false
                }
            }
        }


    }
}
