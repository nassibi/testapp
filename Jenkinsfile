pipeline {
    agent any

    environment {
        NPM_CACHE_DIR = '/.npm'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the repository
                    checkout scm
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    // Pulling and running the Docker container
                    def dockerImage = 'node:23'
                    def dockerRun = docker.image(dockerImage)
                    
                    dockerRun.withRun("-v ${pwd()}:/workspace -w /workspace") {
                        // Commands inside the Docker container
                        sh 'ls -la' // List files to check
                        sh 'node --version' // Check Node version
                        sh 'npm --version' // Check npm version
                        sh 'npm config get cache' // Check npm cache
                    }
                }
            }
        }

        stage('Clean npm cache') {
            steps {
                script {
                    echo 'Cleaning npm cache'
                    sh 'npm cache clean --force'
                }
            }
        }

        stage('Remove node_modules') {
            steps {
                script {
                    echo 'Removing node_modules directory'
                    sh 'rm -rf node_modules'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies using npm ci'
                    sh 'npm ci'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Assuming the project has build and test scripts
                    echo 'Running build and tests'
                    sh 'npm run build'
                    sh 'npm test'
                }
            }
        }

        stage('Docker Cleanup') {
            steps {
                script {
                    echo 'Cleaning up Docker container'
                    sh 'docker system prune -f'
                }
            }
        }
    }

    post {
        always {
            // Clean up the Docker container, in case it was left running
            echo 'Cleaning up Docker container if necessary'
            sh 'docker ps -a -q | xargs -n 1 docker rm -f'
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
