stage('Push to Docker Hub') {
    steps {
        script {
            def branch = env.BRANCH_NAME
            def imageName = branch == 'main' ? 'your-dockerhub-username/nodemain:v1.0' : 'your-dockerhub-username/nodedev:v1.0'

            // Log in to Docker Hub
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
