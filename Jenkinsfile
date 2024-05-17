pipeline {
    agent any

    environment {
        SSH_KEY = credentials('KEYVUE') // GitHub secret name for SSH key
        EC2_INSTANCE = '50.18.136.33' // EC2 instance IP or hostname
        REMOTE_DIR = '/var/www/vue-template' // Remote directory on the EC2 instance
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/iamwaleedmirza/vue-template.git']]])
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Deploy') {
            steps {
                // Copy files to the EC2 instance
                sh "scp -i ${SSH_KEY} -r ./* ubuntu@${EC2_INSTANCE}:${REMOTE_DIR}"

                // Connect to the EC2 instance via SSH
                // sh "ssh -i \${SSH_KEY} ubuntu@\${EC2_INSTANCE} 'cd \${REMOTE_DIR} && pm2 stop app || true'"

                // Install project dependencies on the EC2 instance
                sh "ssh -i \${SSH_KEY} ubuntu@\${EC2_INSTANCE} 'cd \${REMOTE_DIR} && npm install'"

                // Start the application on the EC2 instance
                // sh "ssh -i \${SSH_KEY} ubuntu@\${EC2_INSTANCE} 'cd \${REMOTE_DIR} && pm2 start app'"
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
