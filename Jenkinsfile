pipeline {
    agent any

    environment {
        IMAGE_NAME = "aishwaryacs/netflix-clone"
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AISHWARYACS1236/Netflix-Clone.git'
            }
        }

        stage('Trivy Filesystem Scan') {
            steps {
                sh '''
                    trivy fs --scanners vuln,secret --exit-code 0 .
                '''
            }
        }

        stage('Docker Build') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'tmdb-api-key',
                        variable: 'TMDB_V3_API_KEY'
                    )
                ]) {
                    sh '''
                        docker build \
                        --build-arg TMDB_V3_API_KEY="$TMDB_V3_API_KEY" \
                        -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                        -t ${IMAGE_NAME}:latest \
                        Application-Code
                    '''
                }
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh '''
                    trivy image --severity HIGH,CRITICAL --exit-code 0 ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Docker Hub Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin

                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push ${IMAGE_NAME}:latest

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl set image deployment/netflix-app \
                    netflix-app=${IMAGE_NAME}:${BUILD_NUMBER}

                    kubectl rollout status deployment/netflix-app \
                    --timeout=120s
                '''
            }
        }
    }

    post {
        success {
            echo 'Netflix CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the failed stage in Jenkins console output.'
        }
    }
}
