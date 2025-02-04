pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        skipDefaultCheckout(true)
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/hardeepmty/Jenkins-trial'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                echo "Current directory: $(pwd)"
                echo "Listing files:"
                ls -l
                echo "Navigating to Jenkins-trial directory:"
                cd Jenkins-trial || exit 1
                echo "Listing files in Jenkins-trial:"
                ls -l
                cd node-nginx-app || exit 1
                npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh 'cd Jenkins-trial/node-nginx-app && npm test'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['4db5a6ac-d1ac-47bf-92ad-643c244be927']) { 
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@3.107.228.31 <<EOF
                        cd ~/Jenkins-trial/node-nginx-app
                        git pull origin main
                        npm install
                        pm2 startOrReload server.js --name node-nginx-app || pm2 start server.js --name node-nginx-app
                    EOF
                    '''
                }
            }
        }
    }
}
