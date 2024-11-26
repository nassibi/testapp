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
                    # Temporarily run as root to create the .npm directory
                    sudo mkdir -p /home/node/.npm
                    sudo chown -R node:node /home/node/.npm

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
