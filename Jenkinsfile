pipeline {
    agent any

    environment {
        NODE_VERSION = '18.x'
        SERVICE_ID = 'render-service-id'
        RENDER_API_KEY = 'render-api-key'
    }

    tools {
        nodejs "${NODE_VERSION}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm install'
                        sh 'npm install -g wait-on kill-port'
                    } else {
                        bat 'npm install'
                        bat 'npm install -g wait-on kill-port'
                    }
                }
            }
        }

        stage('Run tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm start &'
                        sh 'wait-on http://localhost:8090'
                        sh 'npm test'
                        sh 'kill-port 8090'
                    } else {
                        bat 'start /b npm run start'
                        bat 'wait-on http://localhost:8090'
                        bat 'npm test'
                        bat 'kill-port 8090'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'CI Pipeline completed.'
        }
        failure {
            script {
                if (isUnix()) {
                    sh 'kill-port 8090 || true'  // Ensure port killing doesn't fail
                } else {
                    bat 'kill-port 8090'
                }
            }
        }
    }
}
