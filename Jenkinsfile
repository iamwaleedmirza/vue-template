pipeline {
    agent any

    environment {
        SSH_CREDENTIALS_ID = 'laravel' // Replace with your credentials ID
        REMOTE_HOST = '50.18.136.33' // Replace with your EC2 instance IP
        REMOTE_USER = 'ubuntu' // Replace with your remote user (e.g., ec2-user, ubuntu)
        REMOTE_DIRECTORY = '/home/ubuntu/node_app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/your-repo/your-node-app.git', branch: 'main' // Replace with your repo details
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: SSH_CREDENTIALS_ID, keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST "
                            cd $REMOTE_DIRECTORY;
                            rm -rf dist; // Remove existing dist folder
                        "
                        // scp -i $SSH_KEY -o StrictHostKeyChecking=no -r dist/* $REMOTE_USER@$REMOTE_HOST:$REMOTE_DIRECTORY/dist/
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST "
                            cd $REMOTE_DIRECTORY;
                            npm install --production;
                            pm2 restart all || pm2 start index.js --name node_app;
                        "
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
