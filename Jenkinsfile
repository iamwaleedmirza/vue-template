

pipeline {
    agent any
    environment {
        EC2_HOST = '50.18.136.33'
        SSH_CREDENTIALS_ID = 'laravel'
    }
    stages {
        stage('SSH to EC2 and create directory') {
            steps {
                script {
                    // SSH into EC2 and create a directory
                    sshCommand remote: [
                        name: 'server',
                        host: "${EC2_HOST}",
                        user: 'ubuntu', // Replace with your EC2 username
                        identityFile: '', // Leave empty if using Jenkins credentials
                        credentialsId: "${SSH_CREDENTIALS_ID}"
                    ], command: 'mkdir -p /home/ubuntu/new_directory'
                }
            }
        }
    }
}

