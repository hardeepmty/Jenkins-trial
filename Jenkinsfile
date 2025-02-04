pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/hardeepmty/Jenkins-trial'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'cd Jenkins-trial/node-nginx-app && npm install'
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
                    ssh ubuntu@3.107.228.31 <<EOF
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
