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
                    # Check versions and directories
                    echo "Listing files before build"
                    ls -la
                    node --version
                    npm --version

                    # Get the npm cache directory path
                    NPM_CACHE_DIR=$(npm config get cache)
                    echo "NPM Cache Directory: $NPM_CACHE_DIR"

                    # Fix npm cache permissions (without sudo)
                    echo "Fixing npm cache permissions"
                    chown -R node:node $NPM_CACHE_DIR

                    # Clean npm cache to avoid potential permission issues
                    echo "Cleaning npm cache"
                    npm cache clean --force

                    # Remove node_modules directory if it exists (to ensure a clean install)
                    echo "Removing node_modules"
                    rm -rf node_modules

                    # Install dependencies using npm ci (clean install)
                    echo "Installing dependencies"
                    npm ci

                    # Run the build command
                    echo "Running build"
                    npm run build

                    # Verify the build output
                    echo "Listing files after build"
                    ls -la
                '''
            }
        }
    }
}
