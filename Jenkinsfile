pipeline {
    agent any

    environment {
        NODE_VERSION = '18.x'
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
                    } else {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Run tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'node ./node_modules/mocha/bin/mocha tests/*.js'
                    } else {
                        bat 'node ./node_modules/mocha/bin/mocha tests/*.js'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'CI Pipeline completed.'
        }
    }
}
