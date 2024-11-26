pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    chown -R node:node /.npm
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}