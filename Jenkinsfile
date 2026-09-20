
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AISHWARYACS1236/Netflix-Clone.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Netflix Clone application...'
            }
        }

        stage('SonarQube') {
            steps {
                echo 'SonarQube analysis will be configured next...'
            }
        }
    }
}
