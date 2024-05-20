pipeline {
    environment {
        IMAGE = "10.100.1.171/muhfalihr/webtemplate"
        DEFAULT_PORT = '80'
        REGISTRY = 'harbor'
    }
    agent { label 'node1' }
    stages {
        stage('Image Tagging') {
            steps {
                script {
                    env.IMAGE_TAG = input message: "Determining Tags for Images", ok: "Yes",
                        parameters: [
                            string(name: 'IMAGE_TAG', defaultValue: '1', description: 'Determining Tags for Images')
                        ]
                }
            }
        }
        stage('Building and Pushing Image') {
            steps {
                script {
                    docker.withRegistry( 'http://10.100.1.171', REGISTRY ) {
                        dockerImage = docker.build( "${IMAGE}:${IMAGE_VERSION}" )
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Run Container with this Image') {
            steps {
                script {
                    env.CONTAINER_NAME = input message: "Container Name", ok: "Yes",
                        parameters: [
                            string(name: 'CONTAINER_NAME', defaultValue: 'container-name', description: 'Container Name')
                        ]
                }
                script {
                    env.PORT = input message: "Port Forwarding for Expose", ok: "Yes",
                        parameters: [
                            string(name: 'PORT', defaultValue: '80', description: 'Port Forwarding for Expose')
                        ]
                }
                script {
                    dockerImage.run("-d -p ${PORT}:${DEFAULT_PORT} --name=${CONTAINER_NAME}")
                }
            }
        }
        stage('Test Docker Container') {
            input {
                message "IP Agent node1"
                ok "Yes"
                parameters {
                    string(name: 'IP_NODE_AGENT', defaultValue: '', description: 'IP Agent node1')
                }
            }
            steps {
                script {
                    sh 'curl http://${IP_NODE_AGENT}:${PORT}'
                }
            }
        }
        stage('Stopping and Remove Container') {
            steps {
                script {
                    sh 'docker container stop ${CONTAINER_NAME}'
                    sh 'docker container rm ${CONTAINER_NAME}'
                }
            }
        }
        stage('Remove Docker Image') {
            steps {
                script {
                    sh 'docker image rm ${IMAGE}:${IMAGE_TAG}'
                }
            }
        }
    }
}
