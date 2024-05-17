pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh """
		docker build -t webtemplate:1 website/
		"""
            }
        }
    }
}
