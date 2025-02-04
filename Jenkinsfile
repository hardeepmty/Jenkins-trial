pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository and explicitly specify the branch
                git branch: 'main', url: 'https://github.com/hardeepmty/Jenkins-trial'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Navigate to the correct directory and install dependencies
                sh 'cd Jenkins-trial/node-nginx-app && npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests in the correct directory
                sh 'cd Jenkins-trial/node-nginx-app && npm test'
            }
        }

        stage('Deploy') {
            steps {
                // Use SSH credentials to deploy to the EC2 server
                sshagent(['4db5a6ac-d1ac-47bf-92ad-643c244be927']) { 
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@3.107.228.31 <<EOF
                        # Navigate to the application directory
                        cd ~/Jenkins-trial/node-nginx-app
                        
                        # Pull the latest changes from the main branch
                        git pull origin main
                        
                        # Install dependencies
                        npm install
                        
                        # Restart the application using PM2 (fallback to start if not running)
                        pm2 startOrReload server.js --name node-nginx-app || pm2 start server.js --name node-nginx-app
                    EOF
                    '''
                }
            }
        }
    }
}
