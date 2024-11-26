pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23'
                    args '--user 1000:1000' // Use the user with UID 1000:1000
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Ensure the .npm directory exists and has proper permissions
                    mkdir -p /home/node/.npm
                    chown -R node:node /home/node/.npm

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
