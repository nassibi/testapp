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
                    # Ensure proper permissions for .npm cache directory
                    mkdir -p /.npm
                    chown -R node:node /.npm

                    # Check versions and directories
                    ls -la
                    node --version
                    npm --version

                    # Install dependencies and build the project
                    npm ci
                    npm run build

                    # Verify the build output
                    ls -la
                '''
            }
        }
    }
}
