pipeline {
    environment {
        image = "10.100.1.171/muhfalihr/test1"
        registryCredential = 'harbor'
        dockerImage = ''
    }
    agent { label 'node1' }
    stages {
        stage('Versioning') {
            steps {
                script {
                    env.IMAGE_VERSION = input message: 'Image versioning...', ok: 'Take this !!',
                            parameters: [
                                string(name: 'Version',
                                defaultValue: '1.',
                                description: 'Image versioning, input to your desire')]
                }
            }
        }
        stage('Building and Pushing Image') {
            steps {
                script {
                    docker.withRegistry( 'http://10.100.1.171', registryCredential ) {
                        dockerImage = docker.build image + ":IMAGE_VERSION"
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
