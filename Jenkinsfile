pipeline {
    agent any

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
                dir('node-nginx-app') {
                    sh '''
                    echo Current directory: $(pwd)
                    echo Installing dependencies...
                    npm install
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Skipping tests for now...'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['4db5a6ac-d1ac-47bf-92ad-643c244be927']) { 
                    sh '''
                    ssh ubuntu@3.107.228.31 <<'EOF'
                        cd ~/Jenkins-trial/node-nginx-app
                        git pull origin main
                        npm install
                        pm2 restart server.js || pm2 start server.js
                    EOF
                    '''
                }
            }
        }
    }
}
