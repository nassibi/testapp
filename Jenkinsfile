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
                    ls -la
                    node --version
                    npm --version
                    chown -R 980:977 "/.npm"
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}
