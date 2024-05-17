pipeline {
    environment {
        image = "10.100.1.171/muhfalihr/test1"
        registryCredential = 'harbor'
        dockerImage = ''
    }
    agent {
        kubernetes {
            yaml """
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: docker
                image: docker:19.03.12
                command:
                - cat
                tty: true
                volumeMounts:
                - name: docker-socket
                  mountPath: /var/run/docker.sock
            volumes:
            - name: docker-socket
              hostPath:
                path: /var/run/docker.sock
            """
        }
    }
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
