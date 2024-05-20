pipeline {
    agent any

    environment {
        SSH_CREDENTIALS_ID = 'laravel' // Replace with your credentials ID
        REMOTE_HOST = '50.18.136.33' // Replace with your EC2 instance IP
        REMOTE_USER = 'ubuntu' // Replace with your remote user (e.g., ec2-user, ubuntu)
        REMOTE_DIRECTORY = '/home/ubuntu/new_directory'
    }

    stages {
        stage('SSH to EC2 and Create Directory') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: SSH_CREDENTIALS_ID, keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST "mkdir -p $REMOTE_DIRECTORY"
                    '''
                }
            }
        }
    }
}
