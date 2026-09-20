```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AISHWARYACS1236/Netflix-Clone.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube-Scanner'

                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=Netflix-Clone \
                            -Dsonar.projectName=Netflix-Clone \
                            -Dsonar.sources=.
                        """
                    }
                }
            }
        }
    }
}
```

