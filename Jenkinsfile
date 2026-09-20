```groovy
pipeline {
    agent any

    tools {
        sonarQube 'SonarQube-Scanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AISHWARYACS1236/Netflix-Clone.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=Netflix-Clone \
                        -Dsonar.projectName=Netflix-Clone \
                        -Dsonar.sources=.
                    '''
                }
            }
        }
    }
}
```
