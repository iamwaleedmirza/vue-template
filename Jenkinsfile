pipeline {
    agent any

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
                withCredentials([string(credentialsId: '50.18.136.33', variable: 'SSH_KEY')]) {
                    // Retrieve the environment variables from GitHub Secrets
                    def ec2Instance = env.EC2_INSTANCE
                    def remoteDir = env.REMOTE_DIR

                    // Copy files to the EC2 instance
                    sh "scp -i $SSH_KEY -r ./* ubuntu@$ec2Instance:$remoteDir"

                    // Connect to the EC2 instance via SSH
                    // sh "ssh -i $SSH_KEY ubuntu@$ec2Instance 'cd $remoteDir && pm2 stop app || true'"

                    // Install project dependencies on the EC2 instance
                    sh "ssh -i $SSH_KEY ubuntu@$ec2Instance 'cd $remoteDir && npm install'"

                    // Start the application on the EC2 instance
                    // sh "ssh -i $SSH_KEY ubuntu@$ec2Instance 'cd $remoteDir && pm2 start app'"
                }
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
