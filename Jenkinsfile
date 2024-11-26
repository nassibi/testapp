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
                    # Ensure the npm directory exists and has proper permissions for the node user
                    mkdir -p /home/node/.npm
                    chown -R node:node /home/node/.npm

                    # Check versions and directories
                    ls -la
                    node --version
                    npm --version

                    # Install dependencies and build the project
                    npm install
                    npm run build

                    # Verify the build output
                    ls -la
                '''
            }
        }
    }
}
