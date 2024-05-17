pipeline {
    environment {
        IMAGE_NAME = "10.100.1.171/muhfalihr/test1"
        REGISTRY_CRED = 'harbor'
        IMAGE_VERSION = "1"
        DOCKER_IMAGE = ''
    }
    agent any
    stages {
        stage('Building and Pushing Image') {
            steps {
                script {
                    docker.withRegistry( 'http://10.100.1.171', REGISTRY_CRED ) {
                        def dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_VERSION}")
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
