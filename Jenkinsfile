pipeline {
    agent any

    environment {
        NODE_VERSION = '18.x'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            if ! command -v node &> /dev/null; then
                                echo "Node.js not found, installing..."
                                curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
                                sudo apt-get install -y nodejs
                            fi
                            node --version
                            npm --version
                        '''
                    } else {
                        bat '''
                            node --version
                            npm --version
                        '''
                    }
                }
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
